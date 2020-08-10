UIImage is also the data type in UIKit that we use to represent things like the icon displayed in this button.
And as I mentioned previously, UIImageView is the class that UIKit provides for displaying a UIImage.
Now, in classical MVC style UIImage can be thought of as a model object.
And UIImageView, of course, as the name implies, is a view.
And these objects in their roles as model and view, have traditional responsibilities.
UIImage is responsible for loading image content.
 And UIImageView is responsible for displaying it, for rendering it.
Now, we can think of this as a simple relationship that we establish once.
 It's a one-way relationship.
Bu the actual story is a little bit more complicated.
In addition to rendering being a continuous process, rather than a one-time event, there's this hidden phase.
It's really important to understand in order to measure the performance of your application.
 And this phase is called decoding.
But in order to discuss decoding, I first need to discuss a concept called a buffer.
A buffer is just a contiguous region of memory.
But we tend to use the term buffer when we're discussing memory that's composed of a sequence of elements of the same size, usually, of the same internal construction.
And for our purposes, one really important kind of buffer is an image buffer.
 This is a term we use for buffer that holds the in-memory representation of some image.
Each element of this buffer describes the color and transparency of a single pixel in our image.
 And consequently, the size of this buffer in memory is proportional to the size of the image that it contains.
One particularly important example of a buffer is called the frame buffer.
And the frame buffer is what holds the actual rendered output of your application.
So, as your application updates its view hierarchy UIKit will render the application's window and all of its subviews into the frame buffer.
And that frame buffer provides per pixel color information that the display hardware will read in order to illuminate the pixels on the display.
Now, that last part happens at a fixed interval.
 It can happen at 60 fps.
 So, every 1/60th of a second.
 Or on an iPad with ProMotion Display, it can happen as fast as every 1/120th of a second.
And if nothing's changed in your application, the display hardware will get the same data back out of the frame buffer that it saw, previously.
But as you change the content of the views in your application, for example, you assign a new UIImage to our image view, here.
UIKit will re-render your application's window into the frame buffer.
 And the next time the display hardware pulls from the frame buffer it'll get your new content.
Now, you can contrast an image buffer to another kind of buffer, a data buffer, which is just a buffer that contains a sequence of bytes.
In our case, we're concerned about data buffers that contain image files.
Perhaps, we've downloaded them from the network or we've loaded them from disk.
A data buffer that contains an image file, typically, begins with some metadata describing the size of the image that's stored in that data buffer.
And then, contains the image data itself, which is encoded in some form like JPEG compression or PNG.
 Which means that the bytes subsequent to that metadata don't, actually, directly describe anything about the pixels in the image.
So, we can take a deeper look at this pipeline that we've set up.
We have a UIImageView here and we've highlighted the region of the frame buffer that will be populated by the image view's rendering.
And we've assigned a UIImage to this image view.
It's got a data buffer that represents the content of an image file.
 Perhaps, downloaded from the network or read from disk.
But we need to be able to populate the frame buffer with per pixel data.
So, in order to do that UIImage will allocate an image buffer whose size is equal to the size of the image that is contained in the data buffer.
 And perform an operation called decoding that will convert the JPEG or PNG or other encoded image data into per pixel image information.
And then, depending on the content mode of our image view.
When UIKit asks the image view to render it will copy and scale the image data from the image buffer as it copies it into the frame buffer.
Now, that decoding phase can be CPU intensive, particularly, for large images.
So, rather than do that work every time UIKit asks the image view to render, UIImage will hang onto that image buffer, so that it only does that work once.
Consequently, your application, for every image that gets decoded, could have a persistent and large memory allocation hanging out.
And this allocation, as I mentioned earlier, is proportional to the size of the input image.
Not necessarily, the size of the image view that's actually rendered in the frame buffer.
 And this can have some pretty negative consequences on performance.
The large allocation that is in your application's address space could force other related content apart from content that it wants to reference.
 This is called fragmentation.
Eventually, if your application starts accumulating a lot of memory usage the operating system will step in and start transparently compressing the content of physical memory.
Now, the CPU needs to be involved in this operation so in addition to any CPU usage in your own application.
You could be increasing global CPU usage that you have no control over.
Eventually, your application could start consuming so much physical memory that the OS needs to start terminating processes.
And it'll start with background processes of low priority.
And, eventually, if your application consumes enough memory, your application itself could get terminated.
And some of those background processes are doing important work on behalf of the user.
So, they might get started up again as soon as they get terminated.
So, even though your application might only be consuming memory for a short period of time, it can have this really long-tail effect on CPU utilization.
So, we want to reduce the amount of memory that our application uses.
 And we can get ahead of the curve with a technique called downsampling.
Now, here we see a little bit more detail about our image rendering pipeline.
Including the fact that the image view we're going to display our image in is actually smaller than the image we're going to display inside of it.
Normally, the core animation framework would be responsible for shrinking that image down during the rendering phase, but we can save some memory by using this downsampling technique.
 And what we're going to do, essentially, is capture that shrinking operation into an object called the thumbnail.
And we're going to wind up with a lower total memory usage, because we're going to have a smaller decoded image buffer.
So, we set up an image source, create a thumbnail, and then capture that decoded image buffer into UIImage.
 And assign that UIImage to our image view.
original data buffer that contained our image.
And we're left with a much smaller long-term memory footprint for our application.
The code to do that has a few steps.
 So, I'm going to walk you through them.
 I'm not going to do extremely low-level detail.
 But I'll highlight the important bits.
First, we're going to create a CGImageSource object.
And CGImageSourceCreate can take an option dictionary.
 And the important option we're going to pass here, is this ShouldCache flag.
And this tells the Core Graphics framework that we're just creating an object to represent the information stored in the file at this URL.
Don't go ahead and decode this image immediately.
 Just create an object that represents.
 We're going to need information from this URL.
Then, we're going to calculate on the horizontal and vertical axis.
going and point size we're going to render at, which is the larger dimension in pixels.
Calculate that information, and then create an options dictionary for our thumbnail.
There are a couple of options listed here.
You can look in the documentation for exactly what these options do.
 But the very important one is this CacheImmediately option.
By passing this option here, we're telling Core Graphics that when I ask you to create the thumbnail that's the exact moment you should create the decoded image buffer for me.
So, we have exact control over when we take that CPU hit for decoding.
Then, we create the thumbnail, which is a CGImage, that we get back.
Wrap that in the UIImage and return it from our helper function that we've written here.
So, to give you an idea of the magnitude of savings that this technique gives us, we're just displaying the full screen image here.
It's 3,000 by 2,000 pixels.
If we do no optimization, just throw UIImageView in the storyboard and assign our image to it, this application takes 31.
5 megabytes just sitting doing nothing.
 Now, using this downsampling technique and only producing an image buffer that's the size of the actual display, we can get the memory usage of this application down to 18.
4 megabytes.
 And that is a huge reduction in memory usage.
Thanks for the applause, but you should all get the applause for implementing this technique in your applications.
You can imagine how much of a big deal this is for an app that's displaying a lot of potentially large input images in a small space on screen.
For example, the Camera Roll.
 You might implement such a view using UICollectionView.
 So, here we've implemented cell for item at indexPath.
 And we're using our helper function that we wrote earlier to downsample the images to the size that they're going to be displayed at when the cell is actually put on the screen.
So, you think this is a pretty good thing to do, right? Like rather than having these large allocations hanging around, we're reducing our memory usage.
Unfortunately, that doesn't save us from another problem that's common in scrollable views like table views and collection views.
It's a, probably seen this before.
 You scroll through an application and it starts hitching as you scroll.
What's happening here is that as we're scrolling the CPU is relatively idle, or the work that it does can be done before the display hardware needs the next copy of the frame buffer.
So, we see fluid motion as the frame buffer is updated and the display hardware is able to get the new frame on time.
But now, we're about to display another row of images.
And we're about to ask Core Graphics to decode those images before we hand the cells back to UICollectionView.
And that could take a lot of CPU time.
So much so, that we don't get around to re-rendering the frame buffer.
But the display hardware is operating on a fixed interval.
So, from the user's perspective the application has just stuttered.
 Now, we're done decoding these images, we're able to provide those cells back to UICollectionView.
And animation continues on, as before.
Just saw a visual hitch, there.
Now, in addition to the obvious responsiveness consequences of this behavior, there's a more subtle detrimental effect on battery life.
Because iOS is very good at managing the power demand on the batter when there is a smooth constant demand on the CPUs.
And what we have here are spikes.
 As new rows are about to come into view on the scroll view, we're spiking the CPU usage.
 And then, returning back down to a low level.
So, there are two techniques we can use to smooth out our CPU usage.
The first one is prefetching.
And if you want to know a whole lot about prefetching check out the A Tour of CollectionView Talk from this year's WWDC.
But the general ideas here, is that prefetching allows CollectionView to inform our data source that it doesn't need a cell right now, but it will in the very near future.
So, if you have any work to do, maybe, you can get a head start.
That allows us spread out CPU usage out over time.
So, we've reduced the maximum size of the CPU usage.
Another technique we can use is performing work in the background.
So, now that we've spread out work over time we can, also, spread it out over available CPUs.
The consequences of this are that your application is more responsive and the device has a longer battery life.
So, to put this in action here, we've got a implementation of the prefetch method on our data source.
And it's going to call our helper function to produce a downsampled version of the image that we're about to display in this CollectionView cell.
And it does this by dispatching work to one of the global asynchronous queues.
Great.
 Our work is happening in the background.
 This is what we wanted to do.
But there is a potential flaw here.
And it's a phenomenon that we like to call thread explosion.
And this is what happens when we ask the system to do more work than there are CPUs available to do it.
 If we're going to display a whole number of images, like 6-8 images at a time, but we're running on a device that only has 2 CPUs, we can't do all of that work at once.
 We can't parallelize over CPUs that don't exist.
Now, to avoid deadlock when we dispatch asynchronously to a global queue, GCD is going to create new threads to capture the work we're asking it to do.
 And then, the CPUs are going to spend a lot of time moving between those threads to try and make incremental progress on all of the work we asked the operating system to do for us.
And switching between those threads, actually, has a pretty significant overhead.
We'd do a lot better if one or more of the CPUs just got a chance to get images out the door.
So, we're going to borrow a technique that was presented last year in the Modernizing Grand Central Dispatch Usage talk.
 And we're going to synchronize some work, or I'm sorry.
 Not synchronize, we're going to serialize some work.
So, rather than simply dispatching work to one of the global asynchronous queues, we're going to create a serial queue.
And inside of our implementation of the prefetch method we're going to asynchronously dispatch to that queue.
 Now, it does mean that an individual image might not start making progress until later than before.
But it also means that the CPU is going to spend less time switching between bits of work that it can do.
 Now, these images that we're displaying can come from a number of places.
They might come with our application, in which case they might be stored in an image asset.
Or they might be stored in a file instead of our application wrapper.
Or they could come from the network.
Or they could be in a document that's stored in the application documents directory.
They could be stored in a cache.
But for artwork that comes with your application, we strongly encourage you to use image assets.
And there are a number of reasons why.
Image assets are optimized for name based and trait-based lookup.
It's faster to look up an image asset in the asset catalog, than it is to search for files on disk that have a certain naming scheme.
The asset catalog runtime has, also, got some really good smarts in it for managing buffer sizes.
And there are, also, some features unrelated to runtime performance that are exclusive to image assets.
Including features like per device thinning, which mean that your application only downloads image resources that are relevant to the device that it's going to run on and vector artwork.
The vector artwork was a feature that was introduced in iOS 11.
 And you enable it by checking the Preserve Vector Data checkbox in the editor for your image asset.
 And the upshot of this is that if your image gets rendered in an image view that is larger or smaller than the native size of the image it doesn't get blurry.
The image is, actually, re-rasterized from the vector artwork so that it has nice crisp edges.
One place that we use this in the operating system.
If you turn on dynamic type to a very large size in the Accessibility settings.
And then you tap and hold on an item in the tab bar a little HUD shows up as a magnified view of the item that you're currently holding your finger over.
So, if you want your artwork to look good in places like this check the Preserve Vector Artwork checkbox in.
 I'm sorry.
 The Preserve Vector Data checkbox in the image asset inspector.
Now, the way this works is very similar to the pipeline we saw before.
 Rather than a decode phase, we have a rasterize phase that's responsible for taking the vector data and turning it into bitmap data that can be copied to the frame buffer.
Now, if we had to do this for all of the vector artwork in your application we would be consuming a lot more CPU.
So, there's an optimization we make here.
If you have an image that has Preserve Vector Data checked, but you render it at the normal size.
 The asset catalog compiler has, actually, already produced a pre-rasterized version of that image and stored it in the asset catalog.
So, rather than doing the complicated math of rasterizing your vector artwork into a bitmap, we can just decode that image that's stored in the asset catalog and render it directly into the frame buffer.
If you're planning on rendering artwork at a few fixed sizes.
 Maybe, you have a small version and a large version of an icon.
Rather than relying on the Preserve Vector Data checkbox, create two image assets that have the two sizes that you know you're going to render your image at.
That will allow the optimization to take the CPU hit of rasterizing your artwork at compile time, rather than every time the image is drawn into the frame buffer.
So, we've seen how to work with UIImage and UIImageView.
But that's not all of the graphical work that your application does.
 Sometimes, your application draws content at runtime.
The example of this happening might be seen in something like this editing view in the Photos application.
The UIButton that's displaying an icon and UIButton can use UIImageView directly.
But UIButton doesn't support the style of this Live button, here, that you can tap to enable or disable the Live Photo.
So, we're going to have to do some work here, ourselves.
 And one implementation of this might be to subclass UIView and implement the draw method.
And this implementation here draws a yellow roundRect, draws some text, and an image on top of it.
Don't recommend this approach for a couple of reasons.
Let's compare this view subclass to our UIImageView.
 Now, as you may already be aware, every UIView is, actually, backed by a CALayer in the Core Animation runtime.
And for our image view, the image view creates the, asks the image to create the decoded image buffer.
And then, hands that decoded image over to CALayer to use as the content of its layer.
For our custom view that overrode draw, it's similar, but slightly different.
 The layers responsible for creating an image buffer to hold the contents of our draw method, and then our view, excuses draw function and populates the contents of that image buffer.
Which is then, copied into the frame buffer as needed by the display hardware.
In order to understand how much this is costing us and why we should, perhaps, pursue alternative ways of implementing this UI.
The backing store that we're using here, the image buffer that's attached to the CALayer, the size of that is proportional to the view that we're displaying.
Now, one new feature and optimization that we have in iOS 12 is that the size of the elements in that backing store will, actually, grow dynamically depending on whether you're drawing any color content.
 And whether that color content is within or outside of the standard color range.
So, if you're drawing wide color content using extended SRGB colors, the backing store will, actually, be larger than the backing store would be if you used only colors within the zero to one range.
Now, in previous versions of iOS, you could set the contents format property on CALayer as a hint to Core Animation saying, ''I know I am not going to need to support wide color content in this view'', or, ''I know I am going to need to support wide color content in this view''.
Now, if you do this, you're actually going to be disabling the optimization that we introduced in iOS 12.
So, check your implementations of layerWillDraw.
Make sure you're not going to accidentally defeat an optimization that could benefit your code when running on iOS 12.
But we can do better than just hinting at whether we need a wide color capable backing store.
total amount of backing storage that our application needs.
 We can do that by refactoring this larger view into smaller subviews.
And reducing or eliminating places that override the draw function.
This will help us eliminate duplicate copies of image data that exist in memory.
 And it will allow us to use optimized properties of UIView that don't require a backing store.
So, as I mentioned, overriding the draw method will require creating a backing store to go with your CALayer.
But some of the properties in UIView can still work, even if you don't override draw.
For example, setting the background color of a UIView doesn't require creating a backing store, unless you're using a pattern color.
So, I recommend not using patterned colors with a background color property on UIView.
Instead, create a UIImageView.
Assign your image to that image view.
And use the functions on UIImageView to set your tiling parameters appropriately.
When we want to clip the corners of that rounded rectangle, we want to use the CALayer cornerRadius property.
Because Core Animation is able to render clipped corners without taking any extra memory allocations.
If we, instead, use the more powerful maskView or maskLayer properties we'd wind up taking in extra allocation to store that mask.
If you have a more complicated background that has transparent areas that can't be expressed by the cornerRadius property, again, consider using a UIImageView.
Store that information in your asset catalog or render it at runtime.
And provide that as an image to the image view, rather than using maskView or maskLayer.
Finally, for that icon, the Live Photo icon, UIImageView is capable of colorizing monochrome artwork without taking any extra allocations.
The first thing you want to do is either check the, not check the checkbox, but set the property in the image asset editor, the render mode property to always template.
Or use the withRenderingMode function on UIImageView to create a UIImage whose rendering mode is always template.
Then, assign that image to an image view and set the tintColor of that image view to the color you want the image to render in.
UIImage, as it's rendering your image to the frame buffer, will apply that solid color during that copy operation.
Rather than having to hold on to a separate copy of your image with your solid color applied to it.
Another optimization built into UIKit provided view, UILabel is able to use 75% less memory when displaying monochrome text than when displaying color text or emojis.
If you want to know more about how this optimization works in detail and how to apply it to your custom subclasses of UIView, check out the iOS Memory Deep Dive session.
Goes into great detail about this backing store format called A8.
Sometimes, you want to render artwork offscreen stored in an image buffer in memory.
 And the class UIKit provides to do that is UIGraphicsImageRenderer.
There's another function that's older; UIGraphicsBeginImageContext.
But please, don't use that.
Because only Graphics Image Renderer is capable of correctly rendering wide color content.
 What you can do in your applications is use UIGraphicsImageRenderer to render to an offscreen place.
And then, use UIImageView to display that, efficiently, on the screen.
Similarly, to the optimization that we've introduced in CALayer backing stores.
We've, also, made UIGraphicsImageRenderer capable of dynamically growing the size of its image buffer, depending on the actions you perform in the actions block.
If you are running your code on a operating system prior to iOS 12, you can use the prefersExtendedRange property on UIGraphicsImageRendererFormat to tell UIKit whether you plan on drawing wide color content or not.
But there's a medium middle ground here.
If you're primarily rendering an image in to a graphic image renderer, that image may use a color space that required values outside of the range of SRGB.
But doesn't, actually, require a larger element size to store that information.
So, UIImage has a image renderer format property that you can use to get a UIGraphicsImageRendererFormat object preconstructed for optimal storage of re-rendering that image.
Lastly, we're going to talk a little bit about how to integrate advanced CPU and GPU technologies that we provide in iOS into your applications.
So, if you've got a lot of advanced processing to do to your images, perhaps, in real time, consider using Core Image.
Core Image is a framework that allows you to create a recipe for processing an image and handle that on the CPU or on the GPU.
If you create a UIImage from a CIImage and hand that to UIImageView, UIImageView will take care to execute that recipe on the GPU.
This is efficient and it keeps the CPU free for doing other work in your application.
In order to use it create your CIImage as normal, and then use the UIImage ciImage initializer.
There are other advanced frameworks for processing and rendering graphical content that are available on iOS, including Metal, Vison, and Accelerate.
And one of the data types that is common among these frameworks is CVPixelBuffer.
And this is a data type that represents a buffer that can be in use or not in use on the CPU or on the GPU.
When constructing one of these pixel buffers make sure to use the best initializer.
 The one that's closest to the representation you have at hand.
Don't unwind any of the decoding work.
It's already been done by the existing UIImage or CGImage representations.
And be careful when moving data between the CPU and the GPU, so that you don't just wind up trading off work between the two.
 You can, actually, get them to execute in parallel.
Finally, check out the Accelerate and simd session for information on how to properly format your buffers for being processed by the Accelerator framework.
So, to summarize a few key points.
Implement prefetch in your table views and collection views, so that you can get some work done in advance and avoid hitching.
Make sure that you're not defeating any optimizations that UIKit is providing to reduce the size of the backing stores associated with your views.
If you're bundling artwork with your applications store it in the asset catalog.
Don't store it in files that are associated with your app.
And finally, if you're rendering the same icons at different sizes don't over-rely on the Preserve Vector Data checkbox.
For more information there is a couple of related sessions, including one about actually investigating your performance problems.
 And we'll also have labs, tomorrow and Friday.
 And if you have any questions, come see us in the labs.
 Thanks for watching.