---
title: Custom Fonts with Google WebFont Loader
author: Clint Berry
layout: post
date: 2010-07-14
url: /2010/custom-fonts-with-google-webfont-loader/
seo_follow:
  - 'false'
seo_noindex:
  - 'false'
categories:
  - jQuery
---
I have recently been working on adding local fonts to my customized version of svg-edit. The idea is to have a font selector with several fonts from my local system. I have been using the recently released [Google WebFont Loader API][1] to load font files from my local server into the website. Google has made it easier than ever. (note: Google will collect stats from your website if you use this API, just like they do if you use any of their services.)
  
<!--more-->


  
The first thing I needed was font files. Not all browsers are alike when using the css @font-face elements. Paul Irish [educated me][2] on that one. So I needed different versions of the font file based on the browser. Luckily, Font Squirrel has a [Font Face Generator][3] which generates all the necessary files and the style sheet to boot. I thought to myself: &#8220;Dang, that was easier than I thought it would be. I think I&#8217;ll donate some money to Font Squirrel for developing that awesome app.&#8221; Then I didn&#8217;t get around to it&#8230; maybe tomorrow.

Once you have all the font files and style sheet up on your server, you can instantiate them with the Google WebFont Loader. Why not just include the style sheet? For a couple of reasons: 1) You can&#8217;t get rid of [FOUT][4] 2) If you are loading multiple fonts (like in a font selector) it would take a long time to load them all up-front (Some of the files are 50k+). For my font selector, I want to be able to load the fonts when the font is selected, then it will be cached on the user&#8217;s computer for all future requests.

To keep track of which fonts were loaded (so as not to load them again with WebFont) I created an array with all the font definitions for my selector (Only three to start). I then created the Smm.loadFont function to load the font. The function takes 3 arguments: 1) The font to load 2) [optional] The callback for when the font has finished loading, and 3) [optional] the callback for WHILE the font is loading.

<pre class="wp-code-highlight prettyprint">var Smm = {
    fontDirectory: &#039;css/fonts/&#039;,
    imageDirectory: &#039;css/fonts/font-images/&#039;    
};

Smm.Fonts = {
    BrushScript: {
        cssFile: &#039;BrushScript.css&#039;,
        imageFile: &#039;brush-script.png&#039;,
        loadType: &#039;custom&#039;,
        loaded: false
    },
    CloisterBlackLight: {
        cssFile: &#039;CloisterBlackLight.css&#039;,
        imageFile: &#039;cloister.png&#039;,
        loadType: &#039;custom&#039;,
        loaded: false
    },
    Cooper: {
        cssFile: &#039;Cooper.css&#039;,
        imageFile: &#039;cooper.png&#039;,
        loadType: &#039;custom&#039;,
        loaded: false
    }
};


Smm.loadFont = function(fontFace, active, loading){
    console.log(&#039;Loading font: &#039; + fontFace);
    if(Smm.Fonts[fontFace][&#039;loaded&#039;] === false){
        Smm.Fonts[fontFace][&#039;loaded&#039;] = true;
        
        WebFont.load({
            custom: { 
                families: [fontFace],
                urls: [Smm.fontDirectory+Smm.Fonts[fontFace][&#039;cssFile&#039;]] 
            },
            loading: loading,
            active: active
        });
    }
    else {
        active();
    }
};
</pre>

I added the imageFile setting to each font so that a picture would be used instead of text. This allows the user to see a preview of the font without downloading it. The last thing I did, was create the HTML for the font loader. 

UPDATE: I integrated the drop-down into svg-edit. Check out a demo here: [**svg-edit with custom font selector**][5]

UPDATE: Get the source code with installation details from my [**github repository**][6]

 [1]: http://code.google.com/apis/webfonts/docs/webfont_loader.html
 [2]: http://paulirish.com/2009/bulletproof-font-face-implementation-syntax/
 [3]: http://www.fontsquirrel.com/fontface/generator
 [4]: http://paulirish.com/2009/fighting-the-font-face-fout/
 [5]: http://clintberry.com/demos/svg-edit-with-fonts/
 [6]: https://github.com/clintberry/svg-font-loader