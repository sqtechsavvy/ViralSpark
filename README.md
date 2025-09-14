ViralSpark 🌟
ViralSpark is a lightweight, Swift-based iOS framework that empowers developers to create viral, share-ready AR content for TikTok, Instagram, and X. Using ARKit, Core ML, and AVFoundation, it auto-suggests trendy AR filters (like capybara ears or neon glows) based on 2025’s hottest social media trends, applies them to photos/videos, and exports clips for instant sharing. Inspired by Apple Design Award winners like Denim and CapWords, ViralSpark is your ticket to building apps that creators love.
Perfect for iOS devs chasing fame, this framework is easy to integrate, CocoaPod-compatible, and ready to make your GitHub repo pop off! 🚀
Features

Trend-Driven Filters: Suggests AR effects based on real-time trends (e.g., #CapybaraChallenge or neon aesthetics). [Mock API included; plug in TikTok/X/OpenAI endpoints for live data.]
AR Magic: Leverages ARKit for face-tracking overlays, perfect for quirky, shareable content.
On-Device AI: Uses Core ML for lightweight, privacy-first filter processing.
Share-Ready Exports: Outputs polished .mp4 clips via AVFoundation for social platforms.
Extensible: Add custom filters (e.g., meme text, retro VHS) with a simple ViralFilter protocol.
2025 Vibes: Built for the era of AI content and AR selfies, with hooks for viral trends.

Demo
Apply a trending capybara ears filter in one tap and share to TikTok!
Getting Started
Prerequisites

Xcode 15+ (iOS 17+ for ARKit 7+ support)
Swift 5.7+
iOS device with A12+ chip for ARKit face tracking
Optional: API keys for TikTok/X/OpenAI for live trend data

Installation

Swift Package Manager:dependencies: [
    .package(url: "https://github.com/sqtechsavvy/ViralSpark.git", .upToNextMajor(from: "1.0.0"))
]


CocoaPods:pod 'ViralSpark', :git => 'https://github.com/sqtechsavvy/ViralSpark.git', :tag => '1.0.0'


Add NSCameraUsageDescription to your app’s Info.plist:<key>NSCameraUsageDescription</key>
<string>We need camera access for AR filters.</string>



Usage
Using ViralSpark in UIKit (Swift)
Integrate ViralSpark into a UIKit-based app with an ARSCNView for AR rendering and a button to trigger viral content creation.
import UIKit
import ViralSpark
import ARKit

class ViewController: UIViewController, ARSessionDelegate {
    var arView: ARSCNView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Set up AR view
        arView = ARSCNView(frame: view.bounds)
        view.addSubview(arView)
        arView.session.delegate = self
        
        // Add button to trigger viral content
        let button = UIButton(type: .system)
        button.setTitle("Go Viral!", for: .normal)
        button.frame = CGRect(x: 20, y: view.bounds.height - 100, width: 100, height: 50)
        button.addTarget(self, action: #selector(makeViral), for: .touchUpInside)
        view.addSubview(button)
        
        // Start AR face tracking
        let configuration = ARFaceTrackingConfiguration()
        arView.session.run(configuration)
    }
    
    @objc func makeViral() {
        ViralSparkManager.shared.createViralContent(from: arView.session, category: "trending") { url in
            guard let url = url else { return }
            let activityVC = UIActivityViewController(activityItems: [url], applicationActivities: nil)
            self.present(activityVC, animated: true)
        }
    }
}

Using ViralSpark in SwiftUI
Wrap ARSCNView in a UIViewRepresentable to use ViralSpark in SwiftUI, and add a button to apply filters and share.
import SwiftUI
import ViralSpark
import ARKit

struct ARView: UIViewRepresentable {
    func makeUIView(context: Context) -> ARSCNView {
        let arView = ARSCNView()
        let configuration = ARFaceTrackingConfiguration()
        arView.session.run(configuration)
        context.coordinator.arView = arView
        return arView
    }
    
    func updateUIView(_ uiView: ARSCNView, context: Context) {}
    
    func makeCoordinator() -> Coordinator {
        Coordinator()
    }
    
    class Coordinator {
        var arView: ARSCNView?
    }
}

struct ContentView: View {
    @State private var showingShareSheet = false
    @State private var videoURL: URL?
    
    var body: some View {
        VStack {
            ARView()
                .frame(maxWidth: .infinity, maxHeight: .infinity)
            
            Button(action: {
                guard let arView = ARView.Coordinator().arView else { return }
                ViralSparkManager.shared.createViralContent(from: arView.session, category: "trending") { url in
                    videoURL = url
                    showingShareSheet = true
                }
            }) {
                Text("Go Viral!")
                    .font(.headline)
                    .padding()
                    .background(Color.blue)
                    .foregroundColor(.white)
                    .clipShape(Capsule())
            }
            .padding(.bottom, 20)
        }
        .sheet(isPresented: $showingShareSheet) {
            if let url = videoURL {
                ShareSheet(activityItems: [url])
            }
        }
    }
}

struct ShareSheet: UIViewControllerRepresentable {
    let activityItems: [Any]
    
    func makeUIViewController(context: Context) -> UIActivityViewController {
        UIActivityViewController(activityItems: activityItems, applicationActivities: nil)
    }
    
    func updateUIViewController(_ uiViewController: UIActivityViewController, context: Context) {}
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

Notes

Assets: Add a placeholder capy_ears image and CapyEars.mlmodel to your app bundle for the capybara filter, or create custom filters via the ViralFilter protocol.
API Integration: Replace the mock API in ViralSparkManager with real endpoints (e.g., TikTok’s /trending or OpenAI’s /chat/completions) for live trend suggestions.
Sharing: The exported .mp4 is optimized for TikTok, Instagram, and X sharing.

Example Filters

CapybaraEarsFilter: Adds adorable capybara ears (2025’s top meme trend).
Extend with your own: Implement the ViralFilter protocol for custom effects.

Roadmap

 Integrate TikTok/X trends API for real-time suggestions.
 Add more filters (e.g., VHS glitch, meme text overlays).
 Support Vision Pro for spatial AR experiences.
 Community-driven filter marketplace.

Contributing
Want to make ViralSpark even hotter? Fork, add filters, or suggest integrations! Open issues/PRs at github.com/sqtechsavvy/ViralSpark. Tag your demos with #ViralSpark on X for a shoutout!
License
MIT License. See LICENSE for details.
Why ViralSpark?
Built for devs who want to ride 2025’s AR and AI wave, ViralSpark makes it dead simple to create apps that creators flock to. Get those GitHub stars, WWDC buzz, and maybe even an Apple Design Award nod. Let’s go viral! 🎉
