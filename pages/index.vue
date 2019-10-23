<template>
  <div>
    <h1>Create An Event</h1>
    <div id="drag-1" class="draggable">
        <textarea id="note"/>
    </div>
    <div id="drag-2" class="draggable">
        <textarea id="note"/>
    </div>
    <div id="drag-3" class="draggable">
        <textarea id="note"/>
    </div>
  </div>
</template>
<script>
import interact from "interactjs";

export default {

    created() {
        interact(".draggable").draggable({
           
            inertia: true,
            restrict: {
                restriction: "parent",
                endOnly: true
            },
            // enable autoScroll
            autoScroll: true,

            // call this function on every dragmove event
            onmove: function dragMoveListener(event) {
                var target = event.target;
                // keep the dragged position in the data-x/data-y attributes
                var x = (parseFloat(target.getAttribute("data-x")) || 0) + event.dx;
                var y = (parseFloat(target.getAttribute("data-y")) || 0) + event.dy;

                // translate the element
                target.style.webkitTransform = target.style.transform =
                    "translate(" + x + "px, " + y + "px)";

                // update the posiion attributes
                target.setAttribute("data-x", x);
                target.setAttribute("data-y", y);
            },
            // call this function on every dragend event
            onend: function(event) {
                // console.log(event);
                console.log(event.target);
                
            }
        })
        .resizable({
            // resize from all edges and corners
            edges: { left: true, right: true, bottom: true, top: true },

            modifiers: [
            // keep the edges inside the parent
            interact.modifiers.restrictEdges({
                outer: 'parent',
                endOnly: true
            }),

            // minimum size
            interact.modifiers.restrictSize({
                min: { width: 100, height: 50 }
            })
            ],

            inertia: true
        })
        .on('resizemove', function (event) {
            var target = event.target
            var x = (parseFloat(target.getAttribute('data-x')) || 0)
            var y = (parseFloat(target.getAttribute('data-y')) || 0)

            // update the element's style
            target.style.width = event.rect.width + 'px'
            target.style.height = event.rect.height + 'px'

            // translate when resizing from top or left edges
            x += event.deltaRect.left
            y += event.deltaRect.top
            
            // console.log(x);
            // console.log(y);

            target.style.webkitTransform = target.style.transform =
                'translate(' + x + 'px,' + y + 'px)'

            target.setAttribute('data-x', x)
            target.setAttribute('data-y', y)
            target.textContent = Math.round(event.rect.width) + '\u00D7' + Math.round(event.rect.height)
        })
    }
}
</script>
<style scoped>
#drag-1,
#drag-2,
#drag-3 {
  width: 30%;
  min-height: 6.5em;
  margin: 10%;
  background-color: #29e;
  color: white;
  border-radius: 0.75em;
  border:1px solid black;
  padding: 4%;
  touch-action: none;
  user-select: none;
  -webkit-transform: translate(0px, 0px);
  transform: translate(0px, 0px);
  /* position:absolute; */
}
textarea {
    background: transparent;
    border:none;
    width:100%;
    height: 100%;
    resize: none;
}
</style>