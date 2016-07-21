#使用 gulp 和 sass 创建 svg 雪碧图
Written by Mike Street on 11th November 2014
(Last updated 15th March 2016)

Following on from our recent blog post about SVG Sprites which gave an introduction and overview to using SVGs in a sprite, this post will outline the processes and tools we use for creating and using an SVG Sprite at Liquid Light.

Creating and maintaining large SVG sprites can be cumbersome and time consuming, so we decided to automate the process. Rather than managing a single large SVG sprite and tracking the coordinates of each icon individually, we wanted to be able to edit each icon and have the creation and co-ordinate generation automated.

In practice this means that we are able to put all our SVG icons into a folder and the SVG sprite (and PNG fallback for IE8) is created and optimised automatically along with a Sass map or names and co-ordinates. By using Sass mixins we are then able to include our sprites by using a very simple bit of code:
##Automating the process
To integrate SVG sprites into our workflow, we decided we wanted a task runner to create the sprite - this meant that we could individually create and update the individual icons without editing and updating the whole image. Gulp is the task runner of choice, running (amongst other things) gulp-svg-sprites. We also wanted the CSS to be created automatically - with the dimensions and background positions calculated upon creating. This gives the advantage of being able to alter an icons dimensions and the CSS updates to reflect this.

By default, the gulp-svg-sprites plugin generates its own CSS, but typo3 has its own classes so we needed a way to create the dimensions and positions as variables, and allow us to use them on existing selectors. For this, we decided to turn to Sass.

Using Sass, the icons are stored in an array - or "map" (find out more about Sass maps). Using some custom mixins, we are able to call on any icon in the sprite and, upon compilation, output the dimensions and background position of each icon.
