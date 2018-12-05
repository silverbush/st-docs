
# Basic interactive page layout

```
<div id="interactiveContent">
    <img id="layout" src="bg image here" data-audio-src="audio src here" />
    <!-- other content without changes here -->
</div>

<div id="dialogDiv">
    ...
</div>

<style>
    ...
</style>

<script>
    //variables
    
    function loadInteractive() {
        // all events goes here
    }
    
    //other functions
    
</script>

```

## changes in js

* change ```$("#audioElement")[0].pause();``` to ```player.pause();```
* change ```nextPage();``` to ```player.next();```
* remove ```$(document).ready(function () { Resize(); });```
* remove ```PlayPauseBtnChange(false);```
* remove ```image onload``` section
