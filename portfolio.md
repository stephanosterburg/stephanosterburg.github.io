---
layout: page
title: Protfolio
featured_image: /assets/images/pages/movies.png
---


The above banner shows all the movies I worked on over the years - see also [IMDb](https://www.imdb.com/name/nm0652339/). Below you will find work samples from movies I worked while at [FRAMESTORE, London](https://www.framestore.com/), as well my demo reel with work from movies I worked on while I was at [Dreamworks Animation](http://www.dreamworksanimation.com).

### Siggraph Paper

Before I forget, here is the siggraph paper[^1] from 2008 I co-wrote with Nathaniel Dirksen and Rob Vogt:
<p><a href="https://stephanosterburg.github.io/assets/data/posts/2019/1839-abstract.pdf" target="_blank">Art-Directable Dynamic-Hair Shells in "Madagascar: Escape 2 Africa"</a> (link opens in new tab)</p>

[^1]:[ACM Digital Library](https://dl.acm.org/citation.cfm?id=1401094)

### Work Samples

<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
.collapsible {
  background-color: #777;
  color: white;
  cursor: pointer;
  padding: 16px;
  width: 100%;
  border: none;
  text-align: left;
  outline: none;
  font-size: 15px;
}

.active, .collapsible:hover {
  background-color: #777;
}

.content {
  padding: 0 18px;
  display: none;
  overflow: hidden;
  background-color: #f1f1f1;
}
</style>
</head>
<body>

<iframe src="https://player.vimeo.com/video/353638267" width="640" height="360" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>


<button class="collapsible">Christopher Robin Work Sample</button>
<div class="content">
    <p><ul type="disc">
        <li>Developed and implemented workflow toolset</li>
        <li>Co-developed default qualoth sim rig, amongst other things defined simulation and collision settings for Tigger and Piglet</li>
        <li>Shot work:</li>
        <ul>
            <li>Fine-tuned qualoth simulation (look, feel, wrinkles etc.)</li>
            <li>Tweaked hair simulation</li>
            <li>Addressed collisions on Pooh: hair, jumper</li>
            <li>Addressed collisions on Eeyore: hair, booklet</li>
        </ul>
    </ul></p>
</div>
<br><br>

<iframe src="https://player.vimeo.com/video/353637915" width="640" height="360" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>


<button class="collapsible">Thor: Ragnarok Work Smaple</button>
<div class="content">
    <p><ul type="disc">
        <li>Created custom muscle self-collision rig for Hulk; feedback to R&D resulted in improved default muscle collision</li>
        <li>Fine-tuned skin and muscle simulation</li>
    </ul></p>
</div>
<br><br>

<iframe src="https://player.vimeo.com/video/178415077" width="640" height="360" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>

<button class="collapsible">DWA Demo Reel Breakdown </button>
<div class="content">
    <p>
        <ul type="disc">
        00:00:00 - 00:33:06
        <br>
        <b>Trolls / Lead Character Technical Director</b>
       <li>Set creative and technical direction</li>
       <li>Oversaw technical development, including writing custom deformation operators</li>
       <li>Oversaw motion system and body deformations work, both technical and aesthetically.</li>
       <li>Setup character template</li>
       <li>Identify show requirements that were not part of the usual standard template</li>
       <li>Character rigged included: Poppy and Branch, and many more</li>

       <br>
       00:33:07 - 00:55:01
       <br>
       <b>Penguins of Madagascar / Lead Character Technical Director</b>
       <li>same tasks as seen above</li>
       <li>Characters rigged: Classified (wolf) and the Penguins (as seen), and others</li>
       <li>Challenges building the penguin rig:</li>
       <ul>
            <li>Wide range of motion, not only in for the legs reaching far up into the belly area, but also the mouth being able to open wide into the chest area.</li>
            <li>Being able to use the rig for the zombie variations. </li>
        </ul>

        <br>
        00:55:02 - 01:14:13
        <br>
        <b>Mr. Peabody & Sherman / Lead Character Technical Director</b>
       <li>Set creative and technical direction</li>
       <li>Oversaw technical development on that show and coordinated the development efforts from R&D for porting an older lattice box deformer to the shows needs </li>
       <li>Oversaw motion system and body deformations work, both technical and aesthetically.</li>
       <li>Setup character template</li>
       <li>Identify show requirements that were not part of the usual standard template</li>
       <li>Rigged the lead characters Mr. Peabody and Sherman</li>

       <br>
       01:14:14 - 01:50:16
       <br>
       <b>Megamind / Lead Character Technical Director</b>
       <li>Rigged Hal/Titan and Roxanne (as seen)</li>
       <li>As well as the female generic characters and many things more.</li>
       <li>Created a custom muscle system for Titan.</li>

       <br>
       01:50:17 - 02:02:14
       <br>
        <b>Kung Fu Panda 3 / Character Technical Director</b>
       <li>Rigged the Monkey (as seen)</li>
       <li>Developed a custom knuckle rocker motion system so that the monkey can rock on his fingers middle section</li>

       <br>
       02:02:15 – 02:16:01
       <br>
       <b>Madagascar 3 / Character Technical Director</b>
       <li>Character rigged: Alex and Carmen (as seen)</li>
       <li>As well as the elephant’s, dogs and many more characters</li>
       </ul>
    </p>
</div>
<br><br>


<script>
    var coll = document.getElementsByClassName("collapsible");
    var i;

    for (i = 0; i < coll.length; i++) {
      coll[i].addEventListener("click", function() {
        this.classList.toggle("active");
        var content = this.nextElementSibling;
        if (content.style.display === "block") {
          content.style.display = "none";
        } else {
          content.style.display = "block";
        }
      });
    }
</script>
