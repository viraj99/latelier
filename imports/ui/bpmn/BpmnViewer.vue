<template>
  <div id="canvas" ref="canvas" />
</template>

<script>
import "bpmn-js/dist/assets/diagram-js.css";
import "bpmn-js/dist/assets/bpmn-font/css/bpmn-codes.css";
import "bpmn-js/dist/assets/bpmn-font/css/bpmn-embedded.css";
import Viewer from "bpmn-js/dist/bpmn-navigated-viewer.production.min";
import "diagram-js-minimap/assets/diagram-js-minimap.css";
import "./minimap-custom.css";
import minimapModule from "diagram-js-minimap";

export default {
  props: {
    processDiagram: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      viewer: null
    };
  },
  watch: {
    processDiagram: {
      immediate: true,
      handler() {
        this.refresh();
      }
    }
  },
  methods: {
    refresh() {
      this.$nextTick(() => {
        if (!this.viewer) {
          this.viewer = new Viewer({
            container: this.$refs.canvas,
            additionalModules: [minimapModule]
          });
        }
        if (this.processDiagram.xml) {
          this.viewer.importXML(this.processDiagram.xml, () => {
            const canvas = this.viewer.get("canvas");
            canvas.zoom("fit-viewport");
          });
        }
      });
    },

    saveSVG(cb) {
      this.viewer.saveSVG(cb);
    }
  }
};
</script>

<style scoped></style>
