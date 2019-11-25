<script>
    import Cleanp from './Cleanp.svelte'

    let raw_html = "\
<style type=\"text/css\">\n\
\n\
p {\n\
    color: red;\n\
    font-size: 3em;\n\
}\n\
\n\
</style>\n\
<center>\n\
    <strong>\n\
        <p>example string</p>\n\
    </strong>\n\
</center>";

    function setSelectionRange(input, selectionStart, selectionEnd) {
        if (input.setSelectionRange) {
            input.focus();
            input.setSelectionRange(selectionStart, selectionEnd);
        }
        else if (input.createTextRange) {
            var range = input.createTextRange();
            range.collapse(true);
            range.moveEnd('character', selectionEnd);
            range.moveStart('character', selectionStart);
            range.select();
        }
    }

    function setCaretToPos (input, pos) {
        setSelectionRange(input, pos, pos);
    }
    const inputId = "rawHtmlinput";

    function newlineAndIndent(e){
        let target = e.target;
        let currentText = target.value;
        let currentPos = target.selectionStart;
        let previousLineStart = currentPos - 2;

        let lineStart = 0;
        let n_spaces = 0;

        for(let i = previousLineStart; i > 0; i--){
            if (currentText[i] === '\n'){
                lineStart = i+1;
                break;
            }
        }
        for(let i = lineStart; i < target.value.length; i++){
            if (currentText[i] != ' '){
                break;
            }
            n_spaces += 1;
        }
        target.value = currentText.slice(0, currentPos) + ' '.repeat(n_spaces) + currentText.slice(currentPos);
        console.log({ n_spaces: n_spaces })
        setCaretToPos(target, currentPos + n_spaces);
    }

    function deleteNewlineAndEmptyLine(e){
        let target = e.target;
        let currentPos = target.selectionStart;
        let value = target.value;
        let toDelete = 0;
        for (var i = currentPos; i < target.value.length; i++){
            if (value[i] !== ' ' && value[i] !== '\n'){
                return;
            }
            if (value[i] === '\n'){
                break;
            }
            toDelete++;
        }
        target.value = value.slice(0, currentPos) + value.slice(currentPos + toDelete);
        setCaretToPos(target, currentPos);
    }

    function handleKeyUp(e){
        let target = e.target;
        let key = e.key;
        if (target.id === inputId){
            switch(key){
                case "Enter":
                    newlineAndIndent(e);
                    break;
                case "Delete":
                    deleteNewlineAndEmptyLine(e);
                default:
                    /* console.log(key); */
                    break;
            }
        }
    }
</script>

<svelte:window on:keyup={handleKeyUp} />

<style>
    textarea {
        /*         text-align: center; */
        width: 80ch;
        font-family: monospace;
        font-size: 1.0em;
        height: 300px;
    }
</style>

<div>
    <div style=" text-align: center; ">
        <h1>Raw html</h1>
        <textarea id={inputId} bind:value={raw_html} />
        <h1>Formatted html</h1>
    </div>
    <Cleanp {raw_html}/>
</div>
