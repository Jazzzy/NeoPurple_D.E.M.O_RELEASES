
# NeoPurple's D.E.M.O.

--------------------------------------

![Imgur](https://github.com/Jazzzy/NeoPurple_D.E.M.O_RELEASES/blob/master/README_RESOURCES/NeoPurpleLogo.png)

--------------------------------------


Here I will try to explain some things that I consider interesting about the work that it is currently in the D.E.M.O. and some things that will be implemented in the future. I plan to keep adding things to this document while implementing them into the game. It's possible that the newer releases may have content that is not yet explained here, don't worry, this document should be updated eventually, or not and I'm a liar.

The releases of the D.E.M.O. can be found [here](https://github.com/Jazzzy/NeoPurple_D.E.M.O_RELEASES/releases) but for some reason GitHub decided to stop sorting them correctly, or at least in the way I expected so you will have to look for the last one. Sorry for giving you work already.

:P

## 1.- Introduction

--------------------------------------
This is a personal project that serves as a platform to show some of the work that I'm able to do by myself. I've loved video-games since I have memory, when I was a kid I never thought about how games were made and even less about maybe making a videogame myself. I just loved to get lost on the lore of the old JRPGs, to be following that legendary pokemon that ran from route to route or to keep slashing the same monsters again and again in the hack 'n' slash that was the hit at some particular time. 

Loving video-games was one of the reasons that got me into studying Computer Science, which I'm finishing this year (2016-2017, if you read this on the future feel free to say hi on twitter or whatever Social Network is famous at the moment). I seemed to be pretty good at it and I tried to learn what I could to, maybe after a while, be able to help in the coding of a game. In the process of looking to learn how to make a game I discovered that I maybe could be able to do some other things related to game making.

Some of the activities that I did when I was a kid proved to be very useful when making videogames. When I was little I spent a lot of time drawing, I loved to look at anime or comics and try to replicate the drawing style. I also have been kind of a musician my whole life. I started rocking the flute (yeah!) at a young age and then picked up the guitar and the bagpipe, both of which I keep playing today. That gave me some soft of foundation in music.

In this moment in my life I felt that I had some unusual array of abilities that allowed me to make a video-game all by myself. Sure, some things will be bad, I cannot draw like a real artist, I cannot compose and play like a real musician and I cannot animate like a real animator. I don't care that much about making a perfect game, I care about making the game that it's in my head and a game that I can say it's mine, something to show to someone and say with confidence that this is what I can do.


## 2.- Inspiration and References for Learning

--------------------------------------

Of course just drawing and making music is not something that can be plugged into a game without some glue to keep it all together and making sense. I had to learn how to color things for a game, how to animate my drawings and how to represent with my songs the thing that was happening on the screen.

### 2.1.- Drawing and Animation

I have learned all that I know about drawing from either doing it myself and the internet. I picked a Pixel Art style for this game because I thought it could speed up the process of making all of the assets. My knowledge about Pixel Art comes mainly from the [/r/PixelArt/](https://www.reddit.com/r/PixelArt/) subreddit. In there I saw people making the same mistakes that I was making and learned from the advice of the best redditors.

As far as animation is concerned I have learned from some tutorials on YouTube like [this one from Mariel Cartwright](https://www.youtube.com/watch?v=Mw0h9WmBlsw) that does a great job at explaining some things that seem basic but that change the way your animations look by a huge amount. When animating my game and specificly the main character I looked into a game called [Hyper Light Drifter](http://www.heart-machine.com/) by Heart Machine, a game that has one of the most beautyful and fluid low-res animation seen to this day. I got the game and spent hours just looking at the character running while trying to draw the frames for my character. The two most notorious things that I learned from that game are the animations for the feet of the character and the waving of the clothing. I also made a similar amount of frames for those animations to try to mimic the fluidity created by their animators.

### 2.2.- Music

The main inspiration for the music in the game (I'm writing this while no music is in the game, 
It's being made, don't worry) is the soundtracks of Neo-Noir films like [Drive](http://www.imdb.com/title/tt0780504/), in fact I'm currently listening to this while I'm writing, I recommend you to put it on to get you on the right mood (You can just click on the image to get to the video).


[![Kavinsky - Nightcall](http://img.youtube.com/vi/MV_3Dpw-BRY/0.jpg)](http://www.youtube.com/watch?v=MV_3Dpw-BRY)


I have also looked at some YouTube channels like [New Retro Wave](https://www.youtube.com/user/NewRetroWave) to get in the right mindset before composing, and of course while drawing or coding, dark retro music seems to be an alright companion when working.


## 3.- CRT Effect

--------------------------------------

The CRT effect used in this game has been implemented thanks to [J. Kyle Pittman](https://twitter.com/PirateHearts) who has helped me a lot with some of the shaders and effects. The list of dumb questions answered by him is endless, sorry for the long streak of e-mails Kyle.

Huge thanks also to Ryan Nelson and his [Pixel Camera 2D](https://github.com/RyanNielson/PixelCamera2D). I saw the way he made his Pixel Perfect setup for Unity and tried to implement it with the CRT effect explained to me by Kyle. I'll now try to explain the best way I can how this works.

### 3.1.- Pixel Perfect Camera

To make the main "Camera" two Unity cameras are used. One is a normal orthographic camera that renders the world at the game resolution (400x300 in this case). This is the camera that will behave as the main camera of the game, following the player and all of those things. The trick is that this camera does not render the output to the screen, the output is rendered to a render texture of the same size (400x300). This render texture is then painted onto a quad that is pointed by another camera.

This second camera is a perspective camera that only sees this quad and It's output is rendered onto the screen. When doing this what we achieve is that the whole game works at low resolution and that all of the sprites and movements and all that are made in units that are equal to a pixel. This way we have a feel similar to old low-res games and TVs because we don't have sub-pixel movements. The sprites all have now one pixel per unit and don't need any scaling, everything looks exactly how it was drawn.

This setup also allows our game to run on any sort of resolution without altering the real in-game resolution because the second "high-res" camera can render in any resolution we want it to, it will be always looking to the quad that shows the game in it's real resolucion.

### 3.2.- CRT Shaders

I have implemented some shaders for the CRT effect **both on the low-res camera and the high-res camera**, depending on the nature of the effect. I'll now try to explain how both kind of shaders work.

#### 3.2.1.- Low-Res Shaders

The first thing that we do with the image is to  **recolor the whole scene **. This is done with a  **Color Correction LookUp shader ** that takes a  **2D image as a LookUp table ** for the colors and maps every color from the game to one of the colors allowed. This is the texture used on my game:

![Imgur](http://i.imgur.com/n3FQBup.png)

This particular texture is generated using [this NES palette generator](http://drag.wootest.net/misc/palgen.html) made by draw but with some modifications to get some new colors used only on the Main Character. The palette generated  **does not have the usual NES colors **, it generates the colors that the old TVs showed when receiving a signal from the NES. The colors changed because the decoder of the system worked in YIQ color space and the whole process of conversion made the final colors look  **darker and desaturated ** on the screen.

After that we apply a shaders that achieve a multitude of effects, I'll start by explaining this one:

![Imgur](http://i.imgur.com/1Q5U09r.png)

![Imgur](http://i.imgur.com/WhJ4Ud1.png)

What we mainly see is these two images is that **red trail left behind** by the character and by those particles when they are moving on the screen. This is achieved by **storing the last frame and blending it with the current one**. This last frame also samples its pixels to the one directly to the left and the right, achieving an horizontal blur and some fuzz. The last frame is also scaled by an RGB value that emphasizes reds just like an old CRT TV would look when the color changed from a bright one to a dark one thanks to the properties of the phosphor in it.


Now we will take a look at the **vertical sharpening effect**.

This is without any sharpening:

![Imgur](http://i.imgur.com/KiOq876.png)

And this is with max sharpening:

![Imgur](http://i.imgur.com/bXPyNWE.png)

What is happening here is caused by sampling some pixels to the right and the left of the current pixel we are working on. Then we calculate the **difference in brightness** between those other pixels and the current one and we **scale the brightness of the current pixel** based in this difference to it's neighbors. The weight of the difference in brightness is **negated every other pixel** to obtain the bright and dark lines seen on the second image.

We will now take a look at the **last effect** on the low-res camera:

(.gif missing right now because it was too big for imgur and GitHub :P)

In the gif we see an effect caused by the way the **NTSC signal** works turned to 11, here we have an explanation from [the nesdev wiki](http://wiki.nesdev.com/w/index.php/NTSC_video#Scanline_Timing):

>This video timing is non-standard. In standard NTSC, a scanline is 227.5 subcarrier cycles long (equivalent to 341.25 NES pixels), and each field is 262 or 263 lines tall with the vertical sync pulse in one frame half a scanline late to make the TV draw the other field of the interlaced frame half a scanline up. The NES and most other pre-Dreamcast consoles draw the fields on top of each other, resulting in a non-standard low-definition "progressive" or "double struck" video mode sometimes called 240p. Some high-definition displays and upscalers cannot handle 240p video, instead introducing artifacts that make the video appear as if it were interlaced. Artemio Urbana's 240p test suite, which has been ported to NES by Damian Yerrick, contains a set of test patterns to diagnose problems with decoding 240p composite video.

The implementation in the game is done with this mask of RGB pixels (scaled in here):

Carefull with those eyes!

![Imgur](http://i.imgur.com/skCtoFT.png)

This mask is used to weight all the pixels in the screen in order to recreate the **NTSC Scanline effect**. This mask is offset by one pixel every other frame to make the screen jitter as seen in the gif. Kyle mentions the possibility of blending the regular and offset mask to achieve a similar effect that does not rely on a constant 60 frames per second. In my case I use the offset as is, without the blending, since the game runs at a constant framerate pretty much always (for now, I can already hear people complaining in a few months :D).


#### 3.2.2.- High-Res Shaders

That is it for the Low-Res Camera, now it's time for the High-Res one. The first thing we do is to apply a **Scanline Mask** to make the usual vertical lines seen on old CRTs. Here I differ quite a bit from the way Kyle did his implementation. Since he used a resolution way lower than mine he could get away with using a big Scanline texture that showed the lines more clearly. I use a mask way smaller in order to simulate a CRT TV with a "higher resolution" since my game is also at a higher resolution. The mask (scaled) is this one:

![Imgur](http://i.imgur.com/eCUEThz.png)

And the effect of the vertical lines made by the pixels can be seen clearly on this image that I've posted before:

![Imgur](http://i.imgur.com/WhJ4Ud1.png)



What I do after that is to apply a **Vignetting Effect** in order to simulate some dark areas and blur on the borders and corners of the screen. Here you can sort of see what I'm talking about:

![Imgur](http://i.imgur.com/Im3DU9b.png)


Lastly I apply a **Fisheye Effect**  in order to get that barrel distortion. Without it the screen would be completely flat since the quad that the camera is recording is also flat. This effect can be seen on all the images that show the whole screen like the last one or this one:

![Imgur](http://i.imgur.com/pAMkYqv.png)


## 4.- Art and Animation

--------------------------------------

The game in general is aesthetically inspired on the **Neo-Noir genre** and so is the style of the art and animation in it. Here I will try to show some of the **sprites and animations** made.

### 4.1.- Main Character

At the time nameless as far as the public is concerned. Our main hero looks like a man in its **30s** and wears a **trench coat** much like a Neo-Noir inspired gangster, a detective or the one and only Columbo. He has a **coffee stain** on his **purple tie** and a nice **black belt** around the coat. This is paired with some nice **old blue jeans** and two **black boots**. To put the icing on the cake he has what at the time was a top of the line **brown hat**.

Some **poses and animations** of this character can be seen here:


![Imgur](http://i.imgur.com/t85IzPL.png)
![Imgur](http://i.imgur.com/Nq0EYzw.png)
![Imgur](http://i.imgur.com/XiJJOQb.png)
![Imgur](http://i.imgur.com/bD6cCg2.png)
![Imgur](https://github.com/Jazzzy/NeoPurple_D.E.M.O_RELEASES/blob/master/README_RESOURCES/runningBack.gif)
![Imgur](https://github.com/Jazzzy/NeoPurple_D.E.M.O_RELEASES/blob/master/README_RESOURCES/runningFront.gif)
![Imgur](https://github.com/Jazzzy/NeoPurple_D.E.M.O_RELEASES/blob/master/README_RESOURCES/runningRight.gif)
![Imgur](https://github.com/Jazzzy/NeoPurple_D.E.M.O_RELEASES/blob/master/README_RESOURCES/runningLeft.gif)

## 5.- Notes

--------------------------------------

Sorry if I make mistakes in English. It's my third language and I don't use it as often as I'd like. Feel free to contact me at @J4zz7 on twitter and tell me something is wrong and I'll change it as soon as I can.
