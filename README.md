# Change-the-height-of-the-model
SwiftUI RealityKit: Change the height of the model
# The first part
![IMG_0635](https://github.com/S-way520/Change-the-height-of-the-model/assets/95877651/590ea065-f28c-4341-aa7e-9818ddf23e5e)
# The second part
Code file 1
```swift
import SwiftUI
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
Code file 2
```swift
import SwiftUI
import RealityKit
struct ContentView : View {
    var body: some View {
        ARViewContainer().edgesIgnoringSafeArea(.all)
    }
}
struct ARViewContainer: UIViewRepresentable {
    func makeUIView(context: Context) -> ARView {
        let arView = ARView(frame: .zero)
        let anchor = AnchorEntity(plane: .horizontal, minimumBounds: [0.2, 0.2])
        arView.scene.addAnchor(anchor)
        let box = MeshResource.generateBox(size: 0.1)
        let boxEntity = ModelEntity(mesh: box)
        anchor.addChild(boxEntity)
        let translationGesture = UIPanGestureRecognizer(target: context.coordinator, action: #selector(Coordinator.didPan(_:)))
        arView.addGestureRecognizer(translationGesture)
        return arView
    }
    func updateUIView(_ uiView: ARView, context: Context) {}
    func makeCoordinator() -> Coordinator {
        Coordinator()
    }
    class Coordinator: NSObject {
        @objc func didPan(_ gesture: UIPanGestureRecognizer) {
            guard let arView = gesture.view as? ARView else { return }
            let translation = gesture.translation(in: arView)
            let boxEntity = arView.scene.anchors[0].children[0]
            switch gesture.state {
            case .began:
                break
            case .changed:
                let deltaPosition = SIMD3<Float>(/*+Float(translation.x)/20000,*/0, -Float(translation.y)/20000, 0)
                boxEntity.position += deltaPosition
            case .ended:
                break
            default:
                break
            }
        }
    }
}
```
