*this is a work in progress*

# Using SVG Icons the Smart Way
The #1 reason we should switch to SVG icons is so we can avoid any browser-specific font-rendering issues without having to apply a bunch of hacky anti-aliasing and normalizing nonsense. SVG is not subject to any font-related hacks or effects. We don't have to worry about weird character mapping issues either. Yay!

##Not your Fathers' SVG
If you're unaware already, SVG is a vector graphic format that supports multiple colors, gradients and can be styled through a bunch of CSS properties and SVG filter effects. With modern techniques you can make them do anything. You can stack them to make more complex icons and artwork. They can also be animated and controlled through javascript. But before you get too excited, all these powers can be yours if you read the primer below and follow some simple directions.

##So many methods, which one is the best?
There are at least six ways to embed SVG in HTML. Most of them lack an adequate method of applying CSS, if at all. Some actually *require* inline CSS which makes baby Jesus kittens cry. Some methods eliminate caching behavior because the resources are embedded in the HTML instead of being stored in external files. Truth be told, there is a lot of old and conflicting information out there on the interwebs about the best way to use SVG icons. I have sifted through all the muck and absorbed as much SVG knowledge as I can handle. After a mighty quest, I have returned with a new spell! 

##The Secret Sauce
The secret is inline <svg> directly in the HTML, with an "icon-xxxxx" class for custom styling through external CSS! Nested inside that is the <use> tag that allows us to select the desired icon sprite from the main spritemap.svg using unique IDs given to each icon. Still with me? Good.

##Efficiency
Our goal here is to put all of our icons in a single spritemap in order to reduce http requests to one for our apps and websites. In some cases we may want to make several spritemaps but in any case the benefits remain. Whereas with icon fonts and PNG fallback methods, we serve and maintain possibly hundreds of asset files, with this method we serve a single maintainable, and easily cacheable XML file.

##But What about Internet Explorer, You Say?

Well supposedly Internet Explorer now supports inline SVG...but you'll be shocked to learn that it doesn't support every method. That said, in most cases, for modern browsers there is no need to even provide PNG fallback images. We can use a tiny javascript polyfill called **svg4everybody.js** that adds support for inline SVG with <use> tags. Soon, even that won't be necessary. 

---------------------------------------------------------------
##Pros
1. You can control style with external CSS (http://css-tricks.com/using-svg/)
2. Reduce http requests
3. Don't have to maintain and serve multiple font files for specific browsers
4. Any dev can easily add a new icon to the project at any time
5. Fewer files to maintain and no need to deal with .png fallback resources that just bloat the project
6. Eliminates font-rendering quirks including character mapping issues
7. Tiny javascript polyfill fixes Internet Explorer fails

##Cons
1. Really old browsers we don't care about can't see them.
2. There may be caching issues on untested platforms like desktop safari. we will fix these issues.
3.Can't use font-related css attributes (this is also kind of a pro, but i needed more cons)
------------------------------------------------
#TL;DR
This process will greatly reduce http requests and eliminate font-rendering quirks. It also cleans up our project by eliminating redundant files, and ultimately streamlines our icon workflow. Now any dev can copy an SVG icon path into the spritemap and start using it immediately with full CSS support! 
---------------------------
#The Workflow
This is intended to be a quick and dirty guide to adding a single icon to your project. Hit me up on chat or email if you have any questions or to complain about my awful documentation skills.

##adding a new icon to the project: 
----------
1. add new .svg to /icon folder (always keep a pristine version in the project)
2. open new icon .svg in text editor, COPY the main shape (path d="..." - ignore empty paths)
3. open 'pg-icons.svg' file in text editor 
4. duplicate the empty <symbol> element right before the </svg> closing tag (ignore the <g> tags, we are substituting <symbol> for reasons outlined here - http://css-tricks.com/svg-symbol-good-choice-icons/)
5. PASTE the path you copied earlier and give it a unique id="" (leave the view-box alone)
6. paste this block in the html where you want to use the icon:

  <svg class="icon-IDNAME"> 
    <use xlink:href="assets/icons/pg-icons.svg#IDNAME"></use>
  </svg>
  (where 'IDNAME' matches the unique id of each symbol)

7. the unique id is only for selecting the icon from the svg spritesheet with the <use> tag in the HTML
8. the icon-IDNAME controls the styling of the icon in CSS/SASS
9. of all the ways to use svg this one makes the most sense because it removes a lot of the complexity and works around some of it's quirks of using svg for this purpose
10. summary: all icons are in a single svg file which limits http requests to one. we retain the ability to cache the spritesheet, and gain the ability the style with external css at the same time. 

##additional notes and guidelines
-------------------------------
* https://github.com/jonathantneal/svg4everybody is a polyfill for using inline SVG in ie8-11.
* source icons come from http://icomoon.io or https://github.com/google/material-design-icons
* 48x48 default size, but can be made any size
* each icon is contained in a <symbol> tag, which together make up the spritesheet (pg-icons.svg)
* each icon is summoned with an <svg> tag containing a <use> tag that points to the id of the desired icon
* "icon-xxxxx" classes are for styling individual icons. they live in sass/_pg-icons.scss, and must be included on the <svg> tag or they won't work.
* SVG has special css attributes and behavior. to change an icon's color use "fill" instead of "color"

todo:
add sample index.html
add sample icons 
add sample css