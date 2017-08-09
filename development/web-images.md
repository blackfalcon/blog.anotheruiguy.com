# What I learned about images and the web

I admit it, I have a print background. As much as I try to leave that all in the past, it always creeps up and bites me in the ass.

Pixel based image asset management is a mystery to many in print and in the new purely digital world. What constitutes a quality asset file to be reproduced? In print there are clear rules, but in digital display, there are many other variables at play. This post is a description of my journey in finally figuring this out.

## The principals of an image

With every image, regardless of intended output, it has three primary attributes; pixel width and height, physical width and height, and dpi. Of course there is color space, color profile and a few others, but that is outside the scope of this discussion.

## Images and print

In the print world, this was all figured out a long time ago. When printing an image there is a direct relationship between the physical size of the image and it's resolution in correlation with the resolution of the printing process, typically referred to as Lines Per Inch, or LPI.

So, you want to print an image at 5" x 5". First, make sure that the image you want to print is actually that physical size. Now for resolution, this all depends on the output process.

For example, if you are printing in newsprint, news paper printers typically use a process consisting of around 100lpi. The standing rule is that you need twice as much dots per inch in your image as the lines per inch you are printing. So for a 5" x 5" 100lpi print, you need a 5" x 5" 200dpi image.

For a full color high end commercial print you may see lines per inch as high as 200lpi. Going with the 2x formula and the previous example, you need a 5" x 5" image with 400dpi to print clearly.

In summary, with physical printing the size of the asset is key and then the resolution is dependent on the output.

## Images in the web

When it came to images on the screen and with the web, the rules are much different. First off, let's talk about 72dpi.

> The choice of 72 PPI by Macintosh for their displays arose from the convenient fact that the official 72 points-per-inch mirrored the 72 pixels-per-inch that actually appeared on their display screens [(1)][1].

But this decision had a negative impact on the rendering of fonts at the poplar 10-point size carried over from typewriters. Microsoft tried to address with the creation of the 96PPI resolution characteristic that is 1/3 times higher then the resolution of the screen. In the end, this addressed the 10-point font issue with Apple, but the result was that 10-point fonts appeared as 13-points.

In time, better technologies, fonts and vector graphics help to address these issues, but in the end we were still left with great confusion as to what the base resolution of a computer is.

### Is it 72dpi or 96dpi?

What we know, 1:96 is a fixed ratio of CSS ‘in’ to CSS ‘px’.  So for drawing graphics on a screen, 1" = 96px. But this is only a measurement, not a resolution.

Where resolution is applied, is wth the screen of your device. With print, resolution is dependent on the output. With images on the screen, the resolution of the device **is** your output. Your computer is either using a standard resolution or a hi-dpi (retina) screen. Now, to add more confusion, a 15.4" Macbook pro with retina display and a 2880 x 1800 native resolution will have a resolution of 220PPI. WAT?! But what about 72dpi or 96dpi? This is where we simply understand that there is no longer a 1:1 between the points-per-inch of the OS and the pixels-per-inch in the display. This is referred to as resolution independence [(4)][4].

To help with this new world of resolution independence, a new measurement spec `dppx` [(2)][2] is a CSS unit representing the number of dots per px unit. So a 96PPI view is == 1dppx, a 192PPI view is == 2dppx. Take note, this is not a physical resolution spec, this is a CSS resolution spec. A standard def device sees CSS as 96dpi and a retina sees CSS at 192dpi. As confusing as this is, at least there is something that is constant as hardware manufactures are continuing to mess with our heads.

This new measurement can be used in a media query as illustrated in this [device pixel density test][px-test].

When it comes to the dpi of an image asset when presented to the screen, dpi is meaningless. I know, crazy! Paint programs such as Photoshop have made us believe that 'web safe' images are 72dpi and I am finding that to be false.

### DPI is meaningless to the web

Here is where this gets a little weird. When it comes to an image you want to present on the web, as I have been saying, dpi has no baring. You can push up an image that has a dpi of 10 and it will look awesome.

What really matters is the pixel width and height of the image. Where the relationship gets weird is that a 800px wide and 1123px tall image at 10dpi has a physical size of 80x 112.3". That's right, the image is huge. If you were to print this at that size, it would look horrible. Good thing we aren't printing it.

This physical size has no relation to the what an inch means on a screen. Like I said, 1" in CSS == 96px or 192px depending on your display. Remember my Macbook pro, it has a physical resolution of 220PPI, 96 and 192 is a CSS measurement, not a physical measurement. What really matters is the 800x1123px.

Taking a 800x1123 image @72dpi, this will produce an image that is 11.111"x15.597". Place that image in a page and then place a CSS block under it with `width: 11.111in;` the CSS block would be wider then the image. Why? It's the 1:96 rule. 11.111in = 1066.656px. What the browser cares about is 800 / 96 = 8.333333in.

#### Examples creating a new file in Photoshop
This whole process gets more interesting when we crate a new image in Photoshop. Again, dpi has no impact on the size of the image. It's physical size that does.

For example, if we have a 800x800px @96dpi it equals 1.83M, but then we increase the dpi to @300dpi, leaving the 800x800px, it still equals 1.83M. It's only when we increase the physical size, be it the inches or the pixel width/height, do we increase file size.

## Definition of screen quality

After all of this, so what does it mean to have an image look good in a computer screen anyway?

We have determined that dpi is meaningless, so save your images at 72dpi or 10dpi, it doesn't matter. For the most part applications like Photoshop will force 72dpi on you as a default setting as a 'web safe' export value.

What matters is the pixel size of the asset. I am finding it a best practice to follow in this new age of retina displays to always start off with art at twice the normal size you intend to display. If you want to display at 400x400px, your asset needs to be 800x800px. Again dpi doesn't matter at export, so don't sweat it.

When including the new asset into the view of your website you will then crunch down the asset to the desired 400x400px size. I know, I know. For how long now we have all been saying that for web optimization we need to serve up the image at the size it will be displayed. Well, just set that on fire, cause times changed.

### But these images are HUGE!

The next concern is that we are now producing large images to send to the client. That's right, we are. But what we have now and we didn't have back then is the increased ability to recreate graphics with CSS3 and much faster networks. Old thinking is old thinking, this is new thinking. When's the last time you sliced up an image to rebuild in a table anyway?

This leaves us with just pictures. What's interesting with this 'squish down' method is we are in effect reducing the size of the image pixel and to match the the device display of hardware pixels. This helps in the reduction of visual artifacts in heavily compressed images.

Let's take for example A 800x1123px poster image. Again, honey badger don't care about dpi. When I export from Photoshop I will set the quality to `0`, yes `0`, this will give me a 70k file. When I bring this into a web page and display at it's native size, it looks pretty bad, especially on a retina device. When I then reduce the image to display at 400px wide, it looks great. If I go down to 300px wide to target a handheld device, it looks even better. Yup, sending the same image to the handled device. I'm pretty crazy like that.

### What about mobile?

But, we can't send these large images to handheld devices, are you mad?! An average handheld device download is around 3 megabit per/sec, that roughly equates to 375k per/sec. At 70k, that's roughly 5 images per second. It is my opinion from these observations that I am not overly concerned with these numbers.

Sure, when you are targeting a handheld device you may want to send a different sized/cropped image, this is all being discussed around the proposed `<picture>` element [(3)][3], but this is still in discussion and nowhere near a recommendation. What I am saying is, if you are in the mindset that you need to send a different resolution/sized image to a handheld device versus the desktop device, you don't have to worry about that.

## Recommendation

At the end of all of this, what is the final recommendation? First off, if the asset you are adding to the page can be reproduced using CSS, do that first. Next, if the asset is iconic look to use ico-fonts or SVGs as these are not pixel dependent assets and will look great on any resolution device.

If you do need to send an image asset, follow these steps:

* Export the asset at twice the pixel width and height you intend to display
* Don't worry about dpi, it doesn't matter
* Use heavy compression when exporting a `.jpg`
* In the HTML attribute or CSS, reduce the size of the image to the desired viewable size.

#### Custom images based in display type

The only caveat I can think of is if the design actually has a different asset based in the client's display type. There are two ways you can do this. First is Webkit's `image-set` function. This function will allow you to set an array of assets with a 1x or 2x designation. Although it is common to see a `_2x` naming convention, it is not required. The key is the `1x` or `2x` within the array.

  .image-set {
    background: -webkit-image-set(url(images/banner_1x.png) 1x, url(images/banner_2x.png) 2x);
    width: 400px;
    height: 40px;
  }

This function is only currently supported by Chrome, Safari and Opera.

The next thing you can do is use a standard media query like the following:

  .image-query {
    background: url(images/banner_1x.png) no-repeat 0 0;
    width: 400px;
    height: 40px;
  }

  @media screen and (min-resolution: 2dppx), (-webkit-min-device-pixel-ratio: 1.3) {
    .image-query {
      background: url(images/banner_2x.png);
      background-size: 100%;
    }
  }

## Conclusion

This research has put me on a wild ride of learning what's happening in the world of image assets. So much of what I used to know and understand about images and print is now dead. This is a whole new way of thinking that has been turned on it's head with the rise of higher resolution devices.

What I thought to be true about 72dpi and it's relationship to monitor displays I am now really finding out to be untrue, or at least outdated thinking.

What makes me feel better is now that I know how this works, it is a lot less work to really think about.

Everything I learned is made available in this [github][gh] repo and [example view][example].





[1]: http://en.wikipedia.org/wiki/Dots_per_inch#Computer_monitor_DPI_standards
[2]: https://developer.mozilla.org/en-US/docs/Web/CSS/resolution
[3]: http://www.w3.org/TR/html-picture-element/
[4]: http://en.wikipedia.org/wiki/Resolution_independence
[px-test]: http://bjango.com/articles/min-device-pixel-ratio/
[gh]: https://github.com/blackfalcon/image-test
[example]: http://www.anotheruiguy.com/hidpi-images/index.html
