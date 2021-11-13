#### Simple Example
<!-- <example> --> 

    function bold (str) {
        return `<b>${str}</b>`;
    }
    
    template = '<%= v | bold %>',
    compiled = overtemplate(template, {filters: {bold: bold}});
    console.log(compiled({v: 'test'}));

<!-- </example> -->
###### Simple Example Output
<!-- <output> --><div style="color:blue;">

    <b>test</b>

</div><!-- </output> --> 
