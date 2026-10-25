<script setup lang="ts">
interface Props {
  type?: "text" | "number";
  minLength?: number;
  min?: number | null;
  max?: number | null;
}
const props = withDefaults(defineProps<Props>(), {
  type: "text",
  minLength: 0,
  min: null,
  max: null,
});
</script>
