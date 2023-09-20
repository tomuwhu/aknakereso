<script>
    const n = 15, asz = 40
    var mbsz = 0, ttsz = 0
    const t = Array(n ** 2 - asz).fill(" ")
                .concat(Array(asz).fill("💣")).sort(() => Math.random() - 0.5)
    var eg = false
    var ta = Array(n).fill(0).map((v, i) => 
                Array(n).fill(0).map((q, j) => t[n * i + j]))
    function f(x, y) {
        if (ta[x][y] == '📍') {
            ta[x][y] == ' '
            return true
        }
        if (ta[x][y] == '💣📍') {
            ta[x][y] == '💣'
            return true
        }
        if (ta[x][y] == '💣') eg = true 
        else {
            let hvm = 0;
            [-1,0,1].forEach(vx => {
                [-1,0,1].forEach(vy => {
                    if (    ta[x+vx] && ta[x+vx][y+vy] && 
                            (ta[x+vx][y+vy] == "💣" || ta[x+vx][y+vy] == "💣📍")
                    ) hvm ++
                })
            })
            ta[x][y] = hvm
            if (hvm == 0) {
                [-1,0,1].forEach(vx => {
                    [-1,0,1].forEach(vy => {
                        if (ta[x+vx] && ta[x+vx][y+vy] 
                            && ta[x+vx][y+vy] == " " 
                        ) f(x + vx, y + vy)
                    })
                })
            }
        }
    }
    function g(x, y, e) {
        if (ta[x][y] == " ") {
            ta[x][y] = '📍'
            ttsz ++
        }
        else if (ta[x][y] == "💣") {
            ta[x][y] = '💣📍'
            mbsz++
        }
        else if (ta[x][y] == "📍") {
            ta[x][y] = ' '
            ttsz --
        }
        else if (ta[x][y] == "💣📍") {
            ta[x][y] = '💣'
            mbsz--
        }
        if (mbsz == asz && ttsz == 0) {
            eg = "Nyert"
        }
        e.preventDefault()
    }
</script>
<h1>Aknakereső</h1>
<table on:contextmenu={e => e.preventDefault()}>
    {#if !eg}
    {#each ta as row, i}
        <tr>
            {#each row as cell, j}
                <td on:click={()=>f(i, j)} 
                    on:contextmenu={e => g(i, j, e)} 
                    class={[0,1,2,3,4,5,6,7].includes(cell) 
                        ? `U${cell}`  
                        : (cell == '📍' || cell == "💣📍" ? "J" : "")}
                    >{cell == '💣' ? '' : (cell == '💣📍' ? '📍' : cell)}</td>
            {/each}
        </tr>
    {/each}
    {:else}
        {#each ta as row, i}
        <tr>
            {#each row as cell, j}
                <td class={`X${cell}`}
                >{cell == "💣📍" ? "📍" : (cell == "📍" ? "H" : cell)}</td>
            {/each}
        </tr>
        {/each}
        {#if eg=="Nyert"}
        <tr><td colspan={n}>Nyert</td></tr> 
        {/if}
    {/if}
</table>
<style>
    h1 {
        color: rgb(77, 23, 23);
        text-shadow: 1px 1px 3px gray;
    }
    td.X💣 {
        background-color: #684734;
    }
    td.X📍 {
        background-color: #b61414;
    }
    td.X💣📍 {
        background-color: #14b62a;
    }
    td.X0,td.X1,td.X2,td.X3,td.X4,td.X5,td.X6,td.X7 {
        text-shadow: 1px 1px 3px gray;
        background-color: rgb(216, 221, 165);
        color:#377674
    }
    td.U5, td.U6, td.U7, td.U8 {
        background-color: #1a3d3c;
        color: rgb(255, 175, 175);
        font-weight: bolder;
    }
    td.U4 {
        background-color: #1f4746;
        color: rgb(123, 233, 255);
        font-weight: bolder;
    }
    td.U3 {
        background-color: #2d5e5c;
        color: rgb(4, 52, 62);
        font-weight: bolder;
    }
    td.U2 {
        background-color: #4b908d;
        color: rgb(4, 52, 62);
        font-weight: bolder;
    }
    td.U1 {
        background-color: #5ca8a6;
        color: rgb(4, 52, 62);
        font-weight: bolder;
    }
    td.U0 {
        background-color: #78b9b7;
        color: #e5e6b8;
    }
    td {
        box-shadow: 1px 1px 4px inset black;
        width: 30px;
        height: 30px;
        background-color: rgb(176, 216, 216);
        border-radius: 7px;
        cursor: pointer;
        color: aliceblue;
    }
    td:hover {
        background-color: rgb(204, 102, 0);
    }
    table {
        border-spacing: 6px;
    }
</style>
