<script>
    let raw_html =
        '<center><strong><p style="font-size: 3em">example string</p></strong></center>';
</script>

<style>
    pre {
        font-size: 2em;
    }
    p {
        font-size: 2em;
    }
    h2 {
        font-size: 2em;
        color: red;
    }
    textarea {
        /*         text-align: center; */
        width: 100ch;
        font-family: monospace;
        font-size: 1.2em;
        height: 200px;
    }
</style>

<div>
    <div style=" text-align: center; ">
        <h1>Raw html</h1>
        <textarea bind:value={raw_html} />
        <h1>Formatted html</h1>
        <pre>
            {@html raw_html}
        </pre>
    </div>
</div>
