<script>
    import Cell from './Cell.svelte';
    import { syllabary, selection, search } from './data.svelte';

    let tones_index = $state([0, 0, 0, 0, 0]);
    let syllable = $derived(
        syllabary.find(s => s.initial == selection.initial && s.final == selection.final)
    );
    $effect(() => {
        if (syllable) tones_index = [0, 0, 0, 0, 0];
    });
    
    const initials = [
        { pin: "m", ipa: "m", zhu: "ㄇ" },  { pin: "n", ipa: "n", zhu: "ㄋ" },
        { pin: "p", ipa: "pʰ", zhu: "ㄆ" }, { pin: "t", ipa: "tʰ", zhu: "ㄊ" },  { pin: "k", ipa: "kʰ", zhu: "ㄎ" },
        { pin: "b", ipa: "p", zhu: "ㄅ" },  { pin: "d", ipa: "t", zhu: "ㄉ" },   { pin: "g", ipa: "k", zhu: "ㄍ" },
        { pin: "c", ipa: "ʦʰ", zhu: "ㄘ" }, { pin: "ch", ipa: "ꭧʰ", zhu: "ㄔ" }, { pin: "q", ipa: "ʨʰ", zhu: "ㄑ" },
        { pin: "z", ipa: "ʦ", zhu: "ㄗ" },  { pin: "zh", ipa: "ꭧ", zhu: "ㄓ" },  { pin: "j", ipa: "ʨ", zhu: "ㄐ" },
        { pin: "f", ipa: "f", zhu: "ㄈ" },  { pin: "s", ipa: "s", zhu: "ㄙ" },   { pin: "sh", ipa: "ʂ", zhu: "ㄕ" }, { pin: "x", ipa: "ɕ", zhu: "ㄒ" }, { pin: "h", ipa: "x", zhu: "ㄏ" },
        { pin: "l", ipa: "l", zhu: "ㄌ" },  { pin: "r", ipa: "ʐ", zhu: "ㄖ" }
    ];
    const finals = [
        { pin: "i", ipa: "ɨ", zhu: "ㄭ" },    { pin: "u", ipa: "u", zhu: "ㄨ" },        { pin: "i", ipa: "i", zhu: "ㄧ" },      { pin: "ü", ipa: "y", zhu: "ㄩ" },
        { pin: "e", ipa: "ɤ", zhu: "ㄜ" },    { pin: "uo", ipa: "wo", zhu: "ㄨㄛ" },    { pin: "ie", ipa: "je", zhu: "ㄧㄝ" },   { pin: "üe", ipa: "ɥe", zhu: "ㄩㄝ" },
        { pin: "ou", ipa: "ou̯", zhu: "ㄡ" },  { pin: "iu", ipa: "jou̯", zhu: "ㄧㄡ" },
        { pin: "ei", ipa: "ei̯", zhu: "ㄟ" },  { pin: "ui", ipa: "wei̯", zhu: "ㄨㄟ" },
        { pin: "en", ipa: "ən", zhu: "ㄣ" },  { pin: "un", ipa: "wən", zhu: "ㄨㄣ" },   { pin: "in", ipa: "in", zhu: "ㄧㄣ" },   { pin: "ün", ipa: "yn", zhu: "ㄩㄣ" },
        { pin: "eng", ipa: "əŋ", zhu: "ㄥ" }, { pin: "ong", ipa: "ʊŋ", zhu: "ㄨㄥ" },   { pin: "ing", ipa: "iŋ", zhu: "ㄧㄥ" },  { pin: "iong", ipa: "jʊŋ", zhu: "ㄩㄥ" },
        { pin: "a", ipa: "a", zhu: "ㄚ" },    { pin: "ua", ipa: "wa", zhu: "ㄨㄚ" },    { pin: "ia", ipa: "ja", zhu: "ㄧㄚ" },
        { pin: "ao", ipa: "au̯", zhu: "ㄠ" },  { pin: "iao", ipa: "jau̯", zhu: "ㄧㄠ" },
        { pin: "ai", ipa: "ai̯", zhu: "ㄞ" },  { pin: "uai", ipa: "wai̯", zhu: "ㄨㄞ" },
        { pin: "an", ipa: "an", zhu: "ㄢ" },  { pin: "uan", ipa: "wan", zhu: "ㄨㄢ" },  { pin: "ian", ipa: "jɛn", zhu: "ㄧㄢ" }, { pin: "üan", ipa: "ɥɛn", zhu: "ㄩㄢ" },
        { pin: "ang", ipa: "aŋ", zhu: "ㄤ" }, { pin: "uang", ipa: "waŋ", zhu: "ㄨㄤ" }, { pin: "iang", ipa: "jaŋ", zhu: "ㄧㄤ" }
    ];
    const specials = [
        { pin: "er", ipa: "ɚ", zhu: "ㄦ" }, { pin: "o", ipa: "o", zhu: "ㄛ" }, { pin: "yo", ipa: "jo", zhu: "ㄧㄛ" }
    ];

    let alphabet = $state('pin');
</script>

<header>
    <div class="radio">
        {#each [{label: '拼音', value: 'pin'}, {label: '音標', value: 'ipa'}, {label: '住音', value: 'zhu'}] as choice}
            <label><input type="radio" name="alphabet" value={choice.value} bind:group={alphabet}/>{choice.label}</label>
        {/each}
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
            <Cell name="initial" id={-1} label="Ø"/>
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
                <Cell name="initial" id={0} label={initials[0][alphabet]}/>
                <Cell name="initial" id={1} label={initials[1][alphabet]}/>
                <div></div>
                <div></div>
                <div></div>
                </div>
                <div class="suprow">
                    <div class="header">塞</div>
                    <div class="row">
                    <div class="header">气</div>
                    <Cell name="initial" id={2} label={initials[2][alphabet]}/>
                    <Cell name="initial" id={3} label={initials[3][alphabet]}/>
                    <div></div>
                    <div></div>
                    <Cell name="initial" id={4} label={initials[4][alphabet]}/>
                    </div>
                    <div class="row">
                    <div class="header">不</div>
                    <Cell name="initial" id={5} label={initials[5][alphabet]}/>
                    <Cell name="initial" id={6} label={initials[6][alphabet]}/>
                    <div></div>
                    <div></div>
                    <Cell name="initial" id={7} label={initials[7][alphabet]}/>
                    </div>
                </div>
                <div class="suprow">
                    <div class="header">塞擦</div>
                    <div class="row">
                    <div class="header">气</div>
                    <div></div>
                    <Cell name="initial" id={8} label={initials[8][alphabet]}/>
                    <Cell name="initial" id={9} label={initials[9][alphabet]}/>
                    <Cell name="initial" id={10} label={initials[10][alphabet]}/>
                    <div></div>
                    </div>
                    <div class="row">
                    <div class="header">不</div>
                    <div></div>
                    <Cell name="initial" id={11} label={initials[11][alphabet]}/>
                    <Cell name="initial" id={12} label={initials[12][alphabet]}/>
                    <Cell name="initial" id={13} label={initials[13][alphabet]}/>
                    <div></div>
                    </div>
                </div>
                <div class="row">
                <div class="header">擦</div>
                <Cell name="initial" id={14} label={initials[14][alphabet]}/>
                <Cell name="initial" id={15} label={initials[15][alphabet]}/>
                <Cell name="initial" id={16} label={initials[16][alphabet]}/>
                <Cell name="initial" id={17} label={initials[17][alphabet]}/>
                <Cell name="initial" id={18} label={initials[18][alphabet]}/>
                </div>
                <div class="row">
                <div class="header">边</div>
                <div></div>
                <Cell name="initial" id={19} label={initials[19][alphabet]}/>
                <Cell name="initial" id={20} label={initials[20][alphabet]}/>
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
                            <div class="cell">
                                <button onclick={ tones_index[t] = (tones_index[t]+1) % syllable.characters[t].length }>
                                    { syllable.characters[t][tones_index[t]] }
                                </button>
                                <span>{ syllable.pinyin.replace(
                                        /(ch|zh|sh|[mpbfntdczslrqjxkghwy])?(i|u)?([aeiou])(.*)/,
                                        (_, i, m, n, c) => (i||"") + (m||"") + n + ['\u0304', '\u0301', '\u030C', '\u0300', ''][t] + c
                                    )
                                }</span>
                            </div>
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
            <Cell name="final" id={-1} label={specials[0][alphabet]}/>
            <Cell name="final" id={-2} label={specials[1][alphabet]}/>
            <Cell name="final" id={-3} label={specials[2][alphabet]}/>
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
            <Cell name="final" id={0} label={finals[0][alphabet]}/>
            <Cell name="final" id={1} label={finals[1][alphabet]}/>
            <Cell name="final" id={2} label={finals[2][alphabet]}/>
            <Cell name="final" id={3} label={finals[3][alphabet]}/>
            </div>
            <div class="row l2 r1">
            <Cell name="final" id={4} label={finals[4][alphabet]}/>
            <Cell name="final" id={5} label={finals[5][alphabet]}/>
            <Cell name="final" id={6} label={finals[6][alphabet]}/>
            <Cell name="final" id={7} label={finals[7][alphabet]}/>
            </div>
            <div class="row l2 r2">
            <Cell name="final" id={8} label={finals[8][alphabet]}/>
            <div></div>
            <Cell name="final" id={9} label={finals[9][alphabet]}/>
            <div></div>
            </div>
            <div class="row l2 r3">
            <Cell name="final" id={10} label={finals[10][alphabet]}/>
            <Cell name="final" id={11} label={finals[11][alphabet]}/>
            <div></div>
            <div></div>
            </div>
            <div class="row l2 r4">
            <Cell name="final" id={12} label={finals[12][alphabet]}/>
            <Cell name="final" id={13} label={finals[13][alphabet]}/>
            <Cell name="final" id={14} label={finals[14][alphabet]}/>
            <Cell name="final" id={15} label={finals[15][alphabet]}/>
            </div>
            <div class="row l2 r5">
            <Cell name="final" id={16} label={finals[16][alphabet]}/>
            <Cell name="final" id={17} label={finals[17][alphabet]}/>
            <Cell name="final" id={18} label={finals[18][alphabet]}/>
            <Cell name="final" id={19} label={finals[19][alphabet]}/>
            </div>
            <div class="row l3 r1">
            <Cell name="final" id={20} label={finals[20][alphabet]}/>
            <Cell name="final" id={21} label={finals[21][alphabet]}/>
            <Cell name="final" id={22} label={finals[22][alphabet]}/>
            <div></div>
            </div>
            <div class="row l3 r2">
            <Cell name="final" id={23} label={finals[23][alphabet]}/>
            <div></div>
            <Cell name="final" id={24} label={finals[24][alphabet]}/>
            <div></div>
            </div>
            <div class="row l3 r3">
            <Cell name="final" id={25} label={finals[25][alphabet]}/>
            <Cell name="final" id={26} label={finals[26][alphabet]}/>
            <div></div>
            <div></div>
            </div>
            <div class="row l3 r4">
            <Cell name="final" id={27} label={finals[27][alphabet]}/>
            <Cell name="final" id={28} label={finals[28][alphabet]}/>
            <Cell name="final" id={29} label={finals[29][alphabet]}/>
            <Cell name="final" id={30} label={finals[30][alphabet]}/>
            </div>
            <div class="row l3 r5">
            <Cell name="final" id={31} label={finals[31][alphabet]}/>
            <Cell name="final" id={32} label={finals[32][alphabet]}/>
            <Cell name="final" id={33} label={finals[33][alphabet]}/>
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

<style lang="scss">
    header {
        position: relative;
        
        margin-bottom: 1rem;
        height: 3rem;

        display: grid;
        place-items: center;
        
        .radio {
            position: absolute;
            left: 0;

            display: flex;

            label {
                display: grid;
                place-items: center;

                height: 3rem;
                width: 3rem;

                &:hover:not(:has(input:checked)) { cursor: pointer; }
                &:has(input:checked) {
                    background-color: var(--red);
                    color: var(--white);
                }
                input { display: none; }
            }
        }
    }

    #page {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: stretch;
    }

    #left {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;
    }

    $cell-height: 2.5rem;
    $cell-width: 4.5rem;
    %target-header {
        color: var(--white);
        background-color: var(--red);
    }
    %border-base {
        content: "";
        pointer-events: none;
        position: absolute;
        border: var(--border);
        box-sizing: border-box;
    }
    @mixin category-border($anchor, $value, $nrow, $ncol) {
        @extend %border-base;
        #{$anchor}: $value;
        height: $nrow * $cell-height;
        width: $ncol * $cell-width;
    }

    .table {
        display: grid;
        .header {
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .header {
            font-weight: var(--bold);
            position: relative;
            &:hover { @extend %target-header; }
        }
    }

    #initials {
        grid-template: repeat(8, $cell-height) / repeat(6, $cell-width);

        .top { display: contents; }
        .rows {
            display: contents;
            .row { display: contents; }
            .suprow {
                grid-row: span 2;
                grid-column: 1/-1;

                display: grid;
                grid-template: subgrid / repeat(2, calc($cell-width / 2)) repeat(5, $cell-width);

                > .header {
                    grid-row: span 2;
                    writing-mode: vertical-lr;
                }
            }
        }

        .row:has(.cell:hover) .header { @extend %target-header; }
        .suprow {
            &:has(.cell:hover, .row .header:hover) > .header { @extend %target-header; }
            > .header:hover ~ .row .header { @extend %target-header; }
        }

        @for $j from 1 through 5 {
            &:has(.row .cell:nth-child(#{$j+1}):hover) .top :nth-child(#{$j}) {
                @extend %target-header;
            }
        }

        .row .header:hover::after {
            @include category-border(right, -5*$cell-width, 1, 5);
        }
        .suprow > .header:hover::after {
            @include category-border(left, $cell-width, 2, 5);
        }
        .top .header:hover::after {
            @include category-border(top, $cell-height, 7, 1);
        }
    }

    #finals {
        grid-template: repeat(13, $cell-height) / repeat(6, $cell-width);

        .head {
            display: contents;
            :first-child, :nth-child(3) { grid-row: span 2; }
            :nth-child(2) { grid-column: span 4; }
        }
        .top { display: contents; }
        .left, .right {
            grid-row: span 11;
            display: grid;
            grid-template-rows: subgrid;
        }
        .left :not(:first-child) { grid-row: span 5; }
        .right :first-child { grid-row: span 2; }
        .rows {
            grid-row: span 11;
            grid-column: span 4;
            display: grid;
            grid-template: subgrid / subgrid;
            .row { display: contents; }
        }

        .head {
            &:has(:first-child:hover) ~ .left *, 
            &:has(:nth-child(2):hover) ~ .top *,
            &:has(:nth-child(3):hover) ~ .right * {
                @extend %target-header;
            }
        }

        @for $j from 1 through 4 {
            &:has(.row .cell:nth-child(#{$j}):hover) .top :nth-child(#{$j}) {
                @extend %target-header;
            }
        }
        @for $l from 1 through 3 {
            &:has(.row.l#{$l} .cell:hover) .left :nth-child(#{$l}) {
                @extend %target-header;
            }
        }
        @for $r from 1 through 5 {
            &:has(.row.r#{$r} .cell:hover) .right :nth-child(5n+#{$r}) {
                @extend %target-header;
            }
        }

        .top .header:hover::after {
            @include category-border(top, $cell-height, 11, 1);
        }
        .left {
            :first-child:hover::after {
                @include category-border(left, $cell-width, 1, 4);
            }
            :not(:first-child):hover::after {
                @include category-border(left, $cell-width, 5, 4);
            }
        }

        %border-right-1 { @include category-border(right, $cell-width, 1, 4); }
        %border-right-2 { @include category-border(right, $cell-width, 2, 4); }

        .right {
            &:has(:nth-child(5n+1):hover) {
                :first-child {
                    @extend %target-header;
                    &::after { @extend %border-right-2; }
                }
                :nth-child(6) {
                    @extend %target-header;
                    &::after { @extend %border-right-1; }
                }
            }
            @for $r from 2 through 5 {
                &:has(:nth-child(5n+#{$r}):hover) :nth-child(5n+#{$r}) {
                    @extend %target-header;
                    &::after { @extend %border-right-1; }
                }
            }
        }
    }

    #specials {
        grid-template: repeat(4, $cell-height) / $cell-width;

        &:hover .header { @extend %target-header; }
        .header:hover::after {
            @include category-border(top, $cell-height, 3, 1);
        }
    }

    #tones {
        grid-template: $cell-height 2*$cell-height / repeat(5, $cell-width);
        .head, .cells { display: contents; }
        .cell {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;

            :first-child {
                font-size: 2rem;
                font-weight: var(--light);
                cursor: pointer;
                user-select: none;
            }
            :last-child { font-size: 1rem; }
        }

        @for $j from 1 through 5 {
            &:has(.header:nth-child(#{$j}):hover) .cells .cell:nth-child(#{$j}) {
                box-shadow: inset 0 0 0 var(--border-size) var(--red);
            }
        }
    }
</style>