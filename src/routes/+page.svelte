<script>
    const n = 15, asz = 40
    var mbsz = 0, ttsz = 0
    const t = Array(n ** 2 - asz).fill(" ")
                .concat(Array(asz).fill("💣"))
                .sort(() => Math.random() - 0.5)
                .sort(() => Math.random() - 0.5)
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
                        ? `U U${cell}`  
                        : (cell == '📍' || cell == "💣📍" ? "J" : "")}
                    >{cell == '💣' ? '' : (cell == '💣📍' ? '📍' : cell)}</td>
            {/each}
        </tr>
    {/each}
    {:else}
        {#each ta as row, i}
        <tr>
            {#each row as cell, j}
                <td class={`Z X${cell}`}
                >{cell == "💣📍" ? "📍" : (cell == "📍" ? "H" : cell)}</td>
            {/each}
        </tr>
        {/each}
        {#if eg=="Nyert"}
        <tr><td class="X💣📍" colspan={n}>Nyert</td></tr>
        {:else}
        <tr><td class="R" colspan={n}>BUMM</td></tr>
        {/if}
    {/if}
</table>
<style>
    @import url('https://fonts.googleapis.com/css2?family=Fuggles&display=swap');
    h1 {
        width: 574px;
        border-radius: 10px;
        border: solid 6px rgb(15, 92, 106);
        display: inline-block;
        background-color: #a3dacf;
        color: rgb(53, 96, 118);
        text-shadow: 1px 1px 3px gray;
        font-family: 'Fuggles', cursive;
        font-size: 60px;
        margin: 0px;
        box-shadow: 1px 1px 4px inset black;
    }
    td.R {
        background-color: #b61414;
        color: white;
        font-family: 'Fuggles', cursive;
        font-size: 25px;
        text-shadow: 1px 1px 4px white;
    }
    td.X💣 {
        background-color: #e2bca6;
        text-shadow: 0px 0px 9px white;
    }
    td.X📍 {
        background-color: #b61414;
        text-shadow: 1px 1px 3px rgb(0, 0, 0);
    }
    td.X💣📍 {
        background-color: #7cce86;
        text-shadow: 1px 1px 3px white;
    }
    td.X0,td.X1,td.X2,td.X3,td.X4,td.X5,td.X6,td.X7 {
        text-shadow: 1px 1px 3px gray;
        background-color: rgb(216, 221, 165);
        color:#377674
    }
    td.U {
        background-color: #1a3d3c;
        color: rgb(255, 175, 175);
        font-weight: bolder;
        text-shadow: 0px 0px 2px white;
    }
    td.U4 {
        background-color: #1f4746;
        color: rgb(28, 205, 241);
        font-weight: bolder;
    }
    td.U3 {
        background-color: #2d5e5c;
        color: rgb(0, 23, 28);
        font-weight: bolder;
    }
    td.U2 {
        background-color: #4b908d;
        color: rgb(14, 79, 92);
        font-weight: bolder;
    }
    td.U1 {
        background-color: #5ca8a6;
        color: rgb(43, 121, 136);
        font-weight: bolder;
    }
    td.U0 {
        background-color: #78b9b7;
        color: #58998c;
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
    td.Z:hover {
        cursor: default;
        background-color: rgb(176, 216, 216);
    }   
    table {
        border-spacing: 6px;
        background-color: #ddeded;
        border-radius: 10px;
        box-shadow: 1px 1px 3px black;
        padding: 6px;
    }
</style>
