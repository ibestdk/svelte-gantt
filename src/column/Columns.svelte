<script lang="ts">
    /**
     * Container component for columns rendered as gantt body background
     */
    export let columns = [];

    export let columnStrokeWidth = 1;
    export let columnStrokeColor = '#efefef';

    function lineAt(ctx, x) {
        ctx.beginPath();
        ctx.moveTo(x, 0);
        ctx.lineTo(x, 20);
        ctx.stroke();
    }

    function createBackground(columns) {
        const canvas = document.createElement('canvas');
        canvas.width = (columns.length - 1) * columns[0].width;
        canvas.height = 20;

        const ctx = canvas.getContext('2d');
        ctx.shadowColor = "rgba(128,128,128,0.5)";
        ctx.shadowOffsetX = 0;
        ctx.shadowOffsetY = 0;
        ctx.shadowBlur = 0.5;
        ctx.lineWidth = columnStrokeWidth;
        ctx.lineCap = "square";
        ctx.strokeStyle = columnStrokeColor;
        ctx.translate(0.5, 0.5);

        columns.forEach(column => {
            lineAt(ctx, column.left);
        });

        const dataURL = canvas.toDataURL();
        return `url("${dataURL}")`;
    }

    let backgroundImage;
    $: {
        backgroundImage = createBackground(columns.slice(0,5));
    }
</script>

<div class="sg-columns" style="background-image:{backgroundImage};">
	<!-- {#each columns as column}
	<Column left={column.left} width={column.width} />
	{/each} -->
</div>
<style>
    .sg-columns {
        position: absolute;
        height: 100%;
        width: 100%;
        overflow: hidden;

        background-repeat: repeat;
        background-position-x: -1px;
    }
</style>