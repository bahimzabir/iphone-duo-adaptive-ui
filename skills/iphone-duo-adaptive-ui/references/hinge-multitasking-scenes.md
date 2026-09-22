# Hinge, multitasking, scenes, and cameras (iOS 27.1)

Sources: TT-SCENES, TT-CAMERA (chapter summaries + sample code), TT-DESIGN, TT-PREPARE, PREP, and DocC pages verified 2026-09-22. These are outside pure layout but affect how a responsive interface behaves. Code is Apple's, verbatim.

## Hinge state — for interactions, not layout (TT-SCENES 0:49–2:35)

- "SwiftUI provides a new onHingeChange modifier and UIKit provides UIHingeInteraction. Both report the high-level hinge status — closed, partially open, and fully open — as well as continuous updates of the hinge angle."
- "Hinge data is observed live and is ideal for driving interactions or effects. For layout, use the arrangement and region APIs covered in 'Strike a pose with adaptive layouts on iPhone Duo' instead."
- "Check for a non-null hinge, since null indicates a device without one."
- Apple does not publish the angle thresholds that separate the statuses.
- UIKit is fully documented (iOS 27.1 beta): `UIHingeInteraction(updateHandler:)` added with `view.addInteraction(_:)`; the handler receives `(interaction, UIHingeInteraction.Update)`; `update.hinge: UIHinge?` is `nil` "when the interaction leaves a hierarchy that provides hinge updates"; `UIHinge.angle: CGFloat` is "in radians"; `UIHinge.status` is `.closed` / `.partiallyOpen` / `.fullyOpen` / `.unknown`; `isEnabled` toggles the interaction. DocC: "The rate and granularity of angle updates are system policy and can change based on system state, so don't depend on a particular update frequency or precision. If you only need to know whether the hinge is closed, partially open, or fully open, prefer `status` over the angle."
- The SwiftUI `onHingeChange` modifier still had no DocC page on 2026-09-22; its shape is known only from the Tech Talk sample below.

```swift
// UIKit — DocC sample for UIHingeInteraction
override func viewDidLoad() {
    super.viewDidLoad()

    let interaction = UIHingeInteraction { [weak self] _, update in
        guard let self else { return }
        // A nil `hinge` indicates the interaction has left a
        // hierarchy that provides hinge updates.
        guard let hinge = update.hinge else {
            handleHingeUnavailable()
            return
        }

        updateAngleDisplay(with: hinge.angle)
        updateStatusDisplay(with: hinge.status)
    }

    view.addInteraction(interaction)
}
```

```swift
// Calculate a pitch bend from the hinge angle  (TT-SCENES 2:17)
struct InstrumentView: View {
    /// Normalized bend, 0 is no bend, 1 is deepest bend
    @State private var pitchBend: Double = 0

    var body: some View {
        GuitarView(pitchBend: pitchBend)
            .onHingeChange { _, context in
                if let hinge = context.hinge, hinge.status == .partiallyOpen {
                    pitchBend = calculatePitchBend(angle: hinge.angle)
                }
                else {
                    pitchBend = 0
                }
            }
    }

    private func calculatePitchBend(angle: Angle) -> Double { ... }
}
```

The closure receives "the previous and current hinge context." Reset your effect in the `else` branch "when the angle isn't being read."

## Split View multitasking (TT-SCENES 2:59; TT-DESIGN; TT-PREPARE)

- "All apps participate in multitasking on iPhone Duo, where two apps sit side by side. If your app already supports resizing on iPad or iPhone mirroring, you're off to a great start."
- A "new layout stacking video and apps together" (pinned picture-in-picture) is handled "the same way, using tools like size classes and scene geometry."
- Bars go to each app's outer edge, so vertical bars can be on the **left** for the left app. Safe areas and layout margins become asymmetric. (TT-PREPARE; HIG-DUO)
- Testing: "Using Device Hub, preview your app on the inner display. Drag your app using the home indicator at the bottom to one side of the screen. An area to drop your app appears. Then, drag your app to the other side. Vertically laid-out content can appear on either side of your app." (TT-PREPARE)
- `UIRequiresFullScreen` is honored, "but your app will still resize when someone opens or closes their iPhone Duo." (TT-PREPARE)

## Multiple scenes (TT-SCENES 3:38)

- "iPhone Duo is the first iPhone to support multiple instances of your app's UI, and apps that support this on iPad will too."
- "new windows can't be created on the outer display — that's reserved for the inner display. Handle errors when requesting new scenes, and use `UIWindowSceneActivationAction`, "which automatically hides when new windows aren't available". DocC name: `UIWindowScene.ActivationAction` (iOS 15+), "a menu element that requests a window scene"; "You can specify an alternate action to display on iPhone and apps that don't support multiple windows.""

## Screens: never `UIScreen.main` (TT-PREPARE 3:57)

- "On a device with two displays, avoid referencing the main screen in your code. It's ambiguous and will be deprecated in a future release. If possible, don't use screen references at all. Instead, use more local concepts like the environment, trait collection, or the scene's bounds. If you need access to the screen, access it dynamically from the window scene."

```swift
// Access the screen from the window scene  (TT-PREPARE 4:16)
let screen = window?.windowScene?.screen
```

```swift
// Replace main screen references  (TT-PREPARE 9:25)
func updateThumbnail(from image: UIImage) {
    // Before
    let screenScale = UIScreen.main.scale

    // After
    let screenScale = traitCollection.displayScale
    // ...
}
```

- Corners: "use Concentricity APIs introduced in iOS 26. They're updated to work with the screen shapes on iPhone Duo. For SwiftUI, use ConcentricRectangle. For UIKit, use UICornerConfiguration."

```swift
// Match the screen corners with Concentricity  (TT-PREPARE 4:30)
// SwiftUI
ConcentricRectangle()
    .fill(Color.green)
    .padding(8.0)
    .ignoresSafeArea()

// UIKit
// UICornerConfiguration
```

## Scene accessories — content on the other display (TT-SCENES 4:22–6:44; PREP; DocC)

DocC (`sceneAccessory(content:)`): "A scene accessory declares supplementary content that the system presents on the app's behalf when an associated piece of system functionality becomes available... The app declares what content to provide; the system decides when and where to present it. Scene accessories enhance the app's experience when available, but the app must remain fully functional without them." `CameraCaptureAccessory` "may be presented while the app is in the foreground and has an active camera capture session... Unlike `ExternalNonInteractiveAccessory`, the content can be interactive." UIKit: `UISceneAccessory.cameraCapture(sceneConfiguration:userInfo:)` registered with `UIViewController.registerSceneAccessory(_:)` (iOS 27.0).

Apple's article "Registering a camera capture accessory on iPhone Duo": "Keep any interaction minimal. Anything your app shows on the outer display is an enhancement... Keep all essential controls in your capture interface, because the system can withdraw accessory content at any time." "Design your capture interface so it works when no outer display exists, and when the system presents nothing there."


- "Scene accessories let your app show content on multiple displays at once, pairing additional content with your main UI." Availability is system-controlled; "they're enabled by default but can be toggled at any time, so respond to availability changes using observation tracking."
- **CameraCaptureAccessory** (camera apps): "pairs additional UI on the outer display while your main UI stays on the inner display... It's available when your app is full screen on the inner display with an active camera session, and you register it on the same view as your camera UI."

```swift
// Observe accessory availability  (TT-SCENES 6:25)
struct CameraRootView: View {
    @State private var model = TeleprompterModel()

    var body: some View {
        CameraView(model: model)
            .sceneAccessory {
                CameraCaptureAccessory(isEnabled: $model.isEnabled) {
                    TeleprompterView(model: model)
                }
                .onAvailabilityChange { newValue in
                    model.isAvailable = newValue
                }
            }
            .toolbar {
                TeleprompterToggle(isEnabled: $model.isEnabled)
                    .disabled(!model.isAvailable)
            }
    }
}
```

## Cameras — what affects UI layout (TT-CAMERA chapter summaries)

Only the parts relevant to interface adaptation are listed; full capture details are in the talk.

- Two front cameras, "both square sensors with an ultrawide field of view." The inner one is "the first under-display camera on iPhone."
- The **virtual front camera** (discovered via `AVCaptureDeviceDiscoverySession`, position `.front`, wide or ultrawide type) "automatically switches between the inner physical camera when open and the outer one when closed."
- Individual devices: `.builtInOuterUltraWideCamera`, `.builtInInnerUltraWideCamera`. "The inner camera supports 1080p up to 60fps, the outer up to 4K and 120fps." The virtual camera exposes only common features.
- Direction: both front cameras report `position == .front`, but "the displays can face opposite directions, so a front camera isn't always looking at you." Use `AVCaptureDeviceDirectionCoordinator` (AVKit) with your `UIView`, the device types to monitor, and a change handler; it reports forward/backward relative to the view's display. One coordinator per display view.
- Preview layout: use `videoGravity` on `AVCaptureVideoPreviewLayer`; when streaming the ultrawide front cameras, "use dynamicAspectRatio on AVCaptureDevice to select a landscape aspect ratio and fill the display."
- Rotation: adopt the rotation coordinator (DocC: `AVCaptureDevice.RotationCoordinator`); "On iPhone Duo, the rotation coordinator will update when your app moves displays." Then set `isCameraSensorOrientationCompensationEnabled = false` on `AVCapturePhotoOutput` for performance.

```swift
// Initialize a direction coordinator  (TT-CAMERA 4:06)
directionCoordinator = AVCaptureDeviceDirectionCoordinator(
    view: view,
    deviceTypes: [
        .builtInOuterUltraWideCamera,
        .builtInInnerUltraWideCamera,
        .builtInDualWideCamera,
    ],
    changeHandler: { [weak self] map in
        self?.updateCameraSession(map)
    }
)
```

The inner-camera **occlusion reserved region** becomes active whenever the camera is active — a camera-centric UI must keep "important content and controls clear of that area" (TT-POSE 0:27). See `arrangement-views-and-reserved-regions.md`.
