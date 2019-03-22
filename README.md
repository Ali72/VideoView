# VideoView
Attempt to build a custom video player using AVFoundation and AVKit. This project was built to enable easier management of reusing the AVPlayer object when recycled throughout a list display (ie: UICollectionView or UITableView).

Some tweaks are needed — the AVPlayer object's KVO (key value observers are not retained properly during runtime for some reason).

## Usage
To set a video's URL:
```

// Get the URL of the remote or local video file path
let videoURL = URL(string: "filepathToYourURL")

// MARK: - VideoView
let videoView = VideoView()
videoView.setItem(url: videoURL)
```

To play or pause the video:
```
// Play the video
videoView.play()

// Pause the video
videoView.pause()
```

When needed to "de-allocate." Typically this method should be called whenever the cells are prepared for reuse.
```
override func prepareForReuse() {
    super.prepareForReuse()
    // MARK: - VideoView
    videoView.deallocate()
}
```

Using the *VideoViewDelegate*. Whenever you want to get the status of the AVPlayer's current AVPlayerItem (or the AVPlayer object's 'currentItem'), set the delegate:
```
videoView.delegate = self
```

Then, set the protocol:
```
// MARK: - VideoViewDelegate
extension UICollectionViewCell: VideoViewDelegate {
    func videoPlaybackStatus(_ item: AVPlayerItem?) {
        guard item?.status == .readyToPlay else {
            print("\(#file) \(#function) - Video is not ready for playback.")
        }
        
        print("Video is ready to play. Do something here.")
    }
}
```

One of the things I want to work on is optimizing this KVO. The key value observer currently determiens whether the status of the player. Perhaps, a better way to determine the deliverability/playback status of the video is by reading its time control status. 

Note: This project will be updaed if I can find more time to work on this...


