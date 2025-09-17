<script>
    import './page.scss';
    import { syllabary, initials, finals, specials } from './syllabary.js';
    import { selection, search } from './data.svelte';
    import Cell from './Cell.svelte';

    let tones_index = $state([0, 0, 0, 0, 0]);
    let syllable = $derived(
        syllabary.find(s => s.initial == selection.initial && s.final == selection.final)
    );
    $effect(() => {
        if (syllable) tones_index = [0, 0, 0, 0, 0];
    });

    let alphabet = $state('pin');
</script>

<header>
    <div class="radio">
        <label><input type="radio" value="pin" bind:group={alphabet}/>拼音</label>
        <label><input type="radio" value="ipa" bind:group={alphabet}/>音標</label>
        <label><input type="radio" value="zhu" bind:group={alphabet}/>住音</label>
    </div>
    <input class=search type=text placeholder="pinyin search..." bind:value={search.value} onkeyup={() => {
        const syl = syllabary.find(s => s.pinyin == search.value);
        if (syl) {
            selection.initial = syl.initial;
            selection.final = syl.final;
        } else {
            selection.initial = undefined;
            selection.final = undefined;
        }
    }}/>
</header>

<div id="page">
    <div id="left">
        <div>
            <h2>声母</h2>
            <div class="table" id="initials">
            <Cell type="initial" id={-1} label="Ø"/>
            <div class="top">
                <div class="header">唇</div>
                <div class="header">龈</div>
                <div class="header">卷舌</div>
                <div class="header">龈腭</div>
                <div class="header">软腭</div>
            </div>
            <div class="rows">
                <div class="row">
                <div class="header">鼻</div>
                <Cell type="initial" id={0} label={initials[0][alphabet]}/>
                <Cell type="initial" id={1} label={initials[1][alphabet]}/>
                <div></div>
                <div></div>
                <div></div>
                </div>
                <div class="suprow">
                    <div class="header">塞</div>
                    <div class="row">
                    <div class="header">气</div>
                    <Cell type="initial" id={2} label={initials[2][alphabet]}/>
                    <Cell type="initial" id={3} label={initials[3][alphabet]}/>
                    <div></div>
                    <div></div>
                    <Cell type="initial" id={4} label={initials[4][alphabet]}/>
                    </div>
                    <div class="row">
                    <div class="header">不</div>
                    <Cell type="initial" id={5} label={initials[5][alphabet]}/>
                    <Cell type="initial" id={6} label={initials[6][alphabet]}/>
                    <div></div>
                    <div></div>
                    <Cell type="initial" id={7} label={initials[7][alphabet]}/>
                    </div>
                </div>
                <div class="suprow">
                    <div class="header">塞擦</div>
                    <div class="row">
                    <div class="header">气</div>
                    <div></div>
                    <Cell type="initial" id={8} label={initials[8][alphabet]}/>
                    <Cell type="initial" id={9} label={initials[9][alphabet]}/>
                    <Cell type="initial" id={10} label={initials[10][alphabet]}/>
                    <div></div>
                    </div>
                    <div class="row">
                    <div class="header">不</div>
                    <div></div>
                    <Cell type="initial" id={11} label={initials[11][alphabet]}/>
                    <Cell type="initial" id={12} label={initials[12][alphabet]}/>
                    <Cell type="initial" id={13} label={initials[13][alphabet]}/>
                    <div></div>
                    </div>
                </div>
                <div class="row">
                <div class="header">擦</div>
                <Cell type="initial" id={14} label={initials[14][alphabet]}/>
                <Cell type="initial" id={15} label={initials[15][alphabet]}/>
                <Cell type="initial" id={16} label={initials[16][alphabet]}/>
                <Cell type="initial" id={17} label={initials[17][alphabet]}/>
                <Cell type="initial" id={18} label={initials[18][alphabet]}/>
                </div>
                <div class="row">
                <div class="header">边</div>
                <div></div>
                <Cell type="initial" id={19} label={initials[19][alphabet]}/>
                <Cell type="initial" id={20} label={initials[20][alphabet]}/>
                <div></div>
                <div></div>
                </div>
            </div>
            </div>
        </div>

        <div>
            <h2>汉子</h2>
            <div class="table" id="tones">
                <div class="head">
                    <div class="header">阴</div>
                    <div class="header">阳</div>
                    <div class="header">上</div>
                    <div class="header">去</div>
                    <div class="header">轻</div>
                </div>
                <div class="cells">
                    {#each [0,1,2,3,4] as t}
                        {#if syllable?.characters[t].length}
                            <button 
                                onclick={ () => {
                                    tones_index[t] = (tones_index[t]+1) % syllable.characters[t].length
                                } }
                            >
                                <span>
                                    { syllable.characters[t][tones_index[t]] }
                                </span>
                                <span>{
                                    syllable.pinyin.replace(
                                        /(ch|zh|sh|[mpbfntdczslrqjxkghwy])?(i|u)?([aeiou])(.*)/,
                                        (_, i, m, n, c) => (i||"") + (m||"") + n + ['\u0304', '\u0301', '\u030C', '\u0300', ''][t] + c
                                    )
                                }</span>
                            </button>
                        {:else}
                            <div></div>
                        {/if}
                    {/each}
                </div>
            </div>
          </div>
    </div>

    <div>
        <h2>他</h2>
        <div class="table" id="specials">
            <div class="header">他</div>
            <Cell type="final" id={-1} label={specials[0][alphabet]}/>
            <Cell type="final" id={-2} label={specials[1][alphabet]}/>
            <Cell type="final" id={-3} label={specials[2][alphabet]}/>
        </div>
    </div>

    <div>
        <h2>韻母</h2>
        <div class="table" id="finals">
        <div class="head">
            <div class="header">腹</div>
            <div class="header">头</div>
            <div class="header">尾</div>
        </div>
        <div class="top">
            <div class="header">Ø</div>
            <div class="header">[w]</div>
            <div class="header">[j]</div>
            <div class="header">[ɥ]</div>
        </div>
        <div class="left">
            <div class="header">闭</div>
            <div class="header">中</div>
            <div class="header">开</div>
        </div>
        <div class="rows">
            <div class="row l1 r1">
            <Cell type="final" id={0} label={finals[0][alphabet]}/>
            <Cell type="final" id={1} label={finals[1][alphabet]}/>
            <Cell type="final" id={2} label={finals[2][alphabet]}/>
            <Cell type="final" id={3} label={finals[3][alphabet]}/>
            </div>
            <div class="row l2 r1">
            <Cell type="final" id={4} label={finals[4][alphabet]}/>
            <Cell type="final" id={5} label={finals[5][alphabet]}/>
            <Cell type="final" id={6} label={finals[6][alphabet]}/>
            <Cell type="final" id={7} label={finals[7][alphabet]}/>
            </div>
            <div class="row l2 r2">
            <Cell type="final" id={8} label={finals[8][alphabet]}/>
            <div></div>
            <Cell type="final" id={9} label={finals[9][alphabet]}/>
            <div></div>
            </div>
            <div class="row l2 r3">
            <Cell type="final" id={10} label={finals[10][alphabet]}/>
            <Cell type="final" id={11} label={finals[11][alphabet]}/>
            <div></div>
            <div></div>
            </div>
            <div class="row l2 r4">
            <Cell type="final" id={12} label={finals[12][alphabet]}/>
            <Cell type="final" id={13} label={finals[13][alphabet]}/>
            <Cell type="final" id={14} label={finals[14][alphabet]}/>
            <Cell type="final" id={15} label={finals[15][alphabet]}/>
            </div>
            <div class="row l2 r5">
            <Cell type="final" id={16} label={finals[16][alphabet]}/>
            <Cell type="final" id={17} label={finals[17][alphabet]}/>
            <Cell type="final" id={18} label={finals[18][alphabet]}/>
            <Cell type="final" id={19} label={finals[19][alphabet]}/>
            </div>
            <div class="row l3 r1">
            <Cell type="final" id={20} label={finals[20][alphabet]}/>
            <Cell type="final" id={21} label={finals[21][alphabet]}/>
            <Cell type="final" id={22} label={finals[22][alphabet]}/>
            <div></div>
            </div>
            <div class="row l3 r2">
            <Cell type="final" id={23} label={finals[23][alphabet]}/>
            <div></div>
            <Cell type="final" id={24} label={finals[24][alphabet]}/>
            <div></div>
            </div>
            <div class="row l3 r3">
            <Cell type="final" id={25} label={finals[25][alphabet]}/>
            <Cell type="final" id={26} label={finals[26][alphabet]}/>
            <div></div>
            <div></div>
            </div>
            <div class="row l3 r4">
            <Cell type="final" id={27} label={finals[27][alphabet]}/>
            <Cell type="final" id={28} label={finals[28][alphabet]}/>
            <Cell type="final" id={29} label={finals[29][alphabet]}/>
            <Cell type="final" id={30} label={finals[30][alphabet]}/>
            </div>
            <div class="row l3 r5">
            <Cell type="final" id={31} label={finals[31][alphabet]}/>
            <Cell type="final" id={32} label={finals[32][alphabet]}/>
            <Cell type="final" id={33} label={finals[33][alphabet]}/>
            <div></div>
            </div>
        </div>
        <div class="right">
            <div class="header">Ø</div>
            <div class="header">[u]</div>
            <div class="header">[i]</div>
            <div class="header">[n]</div>
            <div class="header">[ŋ]</div>
            <div class="header">Ø</div>
            <div class="header">[u]</div>
            <div class="header">[i]</div>
            <div class="header">[n]</div>
            <div class="header">[ŋ]</div>
        </div>
        </div>
    </div>
</div>