# fontdraw
A typewriter art generator for JavaScript

fontdraw - typewriter art using JavaScript 

EXPLANATION:

This is some old, prototype-y code with lots of poorly named scripts (first in P5.js, then PIXI.js)! 
This code “draws” an image with text in whatever font you supply. This gives a “typewriter art look”.

This project has a few basic components. First, how do we get information on the text - like position, pixel information, etc? This is easy enough, surprisingly, because of some nice glyph inspection libraries. I used OpenType.js. Other langauges (like, not JavaScript, because JavaScript is slow!) will likely have equivalents. Using it, we can get the bounding box of each character, and we can loop over each pixel in the bounding box to get its value (nearly any graphical library will have functions that make it easy to retrieve pixel information).

The algorithm itself, then, is a bit like a naive machine learning solution. Just loop this bounding box over the whole image (the 2d array of pixels) and find the minimum error. Stop if you find an error below some predefined threshold (of course, we don’t need to find *two* solutions with error 0).

Then, remove that pixel information from the original image (just subtract the RGB) in the position you’ve decided to place the character. 

Rinse and repeat for every character in your text/story. 

Sounds expensive? It is, and this phototype-y code is not real-time, or fast, nor does it try to be. You can find lots of ways to improve this! 

	See ‘typeWriterBody.js’ for this code!

Second, we want to ‘transition’ between ‘keyframes’ or ‘breakpoints’ - such as, between normal text in a paragraph to that very same text repositioned to draw a face, or a flower, etc. Again, this is surprisingly easy in Javascript using PIXI.js and the DOM.

What the original algorithm outputs is a JSON object for each ‘character’ of your story - it could have position, RGB information, greyscale, opacity, whatever. This is all for placing the text to draw an image.
 We need, then, that same information for the text in the paragraph/story. 

You can simply use HTML to render the text with whatever styling you want - you’re running all these scripts in the browser anyways. Wrap the enclosing parent nodes in some identifier ( such as, id=“characters” ). Then you can use ES6 and the DOM range (https://developer.mozilla.org/en-US/docs/Web/API/Range) to get the position of each character in the DOM and add a “second state” to the aforementioned JSON object. Then save it.

Note, the script that does this is placed In a script tag in the HTML file you put the text/story in (like, in <p> tags, etc).  

Now you can, using whatever graphics library you want, linearly interpolate between each “state” upon key press, mouse click, etc. Whatever fancy event works. 

        See ‘characterize2.js’ and ‘typeWriterAbout.html’ to see exactly how this works!
 
Now, you can obviously do this recursively, and have multiple states, some having just a story, some having just an image, some having both. This is what I wound up doing… however, the way I did it was tedious and copy-paste-y!
