<script>
  import { Polygon } from 'leaflet';

  export default {
    props: [
      'latlngs',
      'color',
      'weight',
      'fill',
      'pane'
    ],
    mounted() {
      // console.log('Polygon mounted is firing');
      const leafletElement = this.$leafletElement = this.createLeafletElement();
      const map = this.$store.state.map.map;
      if (map) {
        // console.log('Polygon adding to map, element:', leafletElement);
        leafletElement.addTo(map);
      }
    },
    destroyed() {
      this.$leafletElement._map.removeLayer(this.$leafletElement);
    },
    render(h) {
      return;
    },
    watch: {
      latlngs(nextLatLngs) {
        // console.log('polygon latlngs changed');
        this.$leafletElement._map.removeLayer(this.$leafletElement);
        const leafletElement = this.$leafletElement = this.createLeafletElement();
        const map = this.$store.state.map.map;
        if (map) {
          // console.log('on update, polygon adding to map, element:', leafletElement);
          leafletElement.addTo(map);
        }
      }
    },
    methods: {
      createLeafletElement() {
        // console.log('Polygon createLeafletElement is firing', this.$props.latlngs);
        return new Polygon(this.$props.latlngs, {
          color: this.$props.color,
          weight: this.$props.weight,
          fill: this.$props.fill,
          pane: this.$props.pane,
          'z-index': 9999,
        });
      },
      parentMounted(parent) {
        // console.log('Polygon parentMounted is firing, this.$leafletElement:', this.$leafletElement);
        const map = parent.$leafletElement;
        this.$leafletElement.addTo(map);
      },
    }
  };

</script>
