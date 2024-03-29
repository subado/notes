---
title: "Tools"
date: 2023-05-22T11:13:37+04:00
draft: true
tags:
  - "js"
---
# Tools

- [Tools](#tools)
  - [Webpack](#webpack)
  - [Vite](#vite)
    - [Slow updates](#slow-updates)
    - [Bundle for Production](#bundle-for-production)
  - [Babel](#babel)
  - [SWC](#swc)

## Webpack

**Bundling** — is the process of combining and optimising source files into files running in a browser.

**Webpack** — is a static module bundler for modern JavaScript applications.  
Its main purpose is to bundle JavaScript files for usage in a browser.

**HMR** — Hot Module Replacement.

Webpack supports HMR.

## Vite

**ESM** — ECMAScript modules.

**Vite** — is a build tool that provide a fast and lean dev experience.  
It consists of two major parts:

- A dev server supporting very fast HMR over ESM.
- A build command that bundles your code with Rollup.

### Slow updates

Vite use **ESM to serves source code** for dev server. This is letting browser take over part of the job of a bundler. Vite only needs to transform and serve source code on demand, as the browser requests it.

<svg viewBox="0 0 1896 1071" fill="none" xmlns="http://www.w3.org/2000/svg">
<text fill="#FFAA3E" xml:space="preserve" style="white-space: pre" font-size="80" letter-spacing="0em"><tspan x="45" y="129.344">Native ESM based dev server</tspan></text>
<rect x="632" y="526" width="273" height="106" rx="10" fill="#C3E88C"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="724.5" y="591.988">entry</tspan></text>
<rect x="1106" y="699" width="274" height="114" rx="10" fill="#666665"></rect>
<g filter="url(#filter0_d_5_61)">
<text fill="#CCCCCB" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0.33em"><tspan x="1213.5" y="768.988">···</tspan></text>
</g>
<rect x="1106" y="346" width="274" height="113" rx="10" fill="#4FC08D"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="1198" y="415.488">route</tspan></text>
<rect x="1102" y="524" width="273" height="114" rx="10" fill="#666665"></rect>
<text fill="#CCCCCB" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="1193.5" y="593.988">route</tspan></text>
<path d="M1101.79 402.463L1067.99 410.054L1091.46 435.529L1101.79 402.463ZM910.168 583.106L1083.96 422.965L1079.9 418.553L906.102 578.693L910.168 583.106Z" fill="#C892E9"></path>
<path d="M1097 581L1067 563.679V598.321L1097 581ZM908 584H1070V578H908V584Z" fill="#999899"></path>
<path d="M1101.79 756.57L1091.2 723.584L1067.93 749.242L1101.79 756.57ZM906.119 583.121L1079.77 740.651L1083.8 736.207L910.151 578.677L906.119 583.121Z" fill="#999899"></path>
<path d="M1552.72 948.839L1545.7 914.916L1519.83 937.953L1552.72 948.839ZM1381.73 761.331L1532.52 930.67L1537 926.68L1386.21 757.341L1381.73 761.331Z" fill="#999899"></path>
<path d="M1549.95 756.569L1541.94 722.868L1516.76 746.659L1549.95 756.569ZM1381.79 582.96L1529.23 739.005L1533.59 734.884L1386.15 578.839L1381.79 582.96Z" fill="#999899"></path>
<path d="M1547.19 585.049L1517.64 566.972L1516.76 601.602L1547.19 585.049ZM1383.89 583.898L1520.12 587.362L1520.27 581.364L1384.04 577.9L1383.89 583.898Z" fill="#999899"></path>
<path d="M1548.57 402.463L1519.02 384.386L1518.14 419.015L1548.57 402.463ZM1385.27 401.312L1521.5 404.776L1521.66 398.778L1385.43 395.314L1385.27 401.312Z" fill="#C892E9"></path>
<path d="M631.489 585.049L601.583 567.567L601.396 602.207L631.489 585.049ZM375.576 586.666L604.473 587.903L604.506 581.903L375.608 580.666L375.576 586.666Z" fill="#C892E9"></path>
<path d="M1549.95 219.877L1516.97 230.462L1542.63 253.735L1549.95 219.877ZM1390.34 400.329L1534.04 241.892L1529.59 237.861L1385.89 396.298L1390.34 400.329Z" fill="#C892E9"></path>
<path d="M1547.19 573.983L1539.89 540.12L1514.21 563.372L1547.19 573.983ZM1385.89 400.327L1526.84 555.983L1531.29 551.956L1390.34 396.3L1385.89 400.327Z" fill="#C892E9"></path>
<rect x="1553" y="152" width="274" height="113" rx="10" fill="#4FC08D"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="1626" y="221.488">module</tspan></text>
<rect x="1553" y="341" width="274" height="114" rx="10" fill="#4FC08D"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="1621.5" y="411.818">module</tspan></text>
<rect x="1550" y="517" width="274" height="114" rx="10" fill="#666665"></rect>
<text fill="#CCCCCB" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="1623" y="586.988">module</tspan></text>
<rect x="1553" y="707" width="274" height="113" rx="10" fill="#666665"></rect>
<text fill="#CCCCCB" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="1626" y="776.488">module</tspan></text>
<rect x="1553" y="896" width="274" height="113" rx="10" fill="#666665"></rect>
<text fill="#CCCCCB" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0.33em"><tspan x="1660.5" y="965.488">···</tspan></text>
<rect x="45" y="491" width="330" height="179" rx="10" fill="#029788"></rect>
<text fill="white" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="154.707" y="570.988">Server
</tspan><tspan x="162.76" y="615.988">ready</tspan></text>
<line x1="507.615" y1="459.201" x2="506.232" y2="569.859" stroke="#C892E9" stroke-width="4" stroke-dasharray="8 8"></line>
<line x1="1038.78" y1="733.073" x2="1037.37" y2="883.845" stroke="#E06666" stroke-width="4" stroke-dasharray="8 8"></line>
<text fill="#E06666" xml:space="preserve" style="white-space: pre" font-size="38" letter-spacing="0em"><tspan x="918" y="938.988">Dynamic import
</tspan><tspan x="918" y="983.988">(code split point)</tspan></text>
<text fill="#C892E9" xml:space="preserve" style="white-space: pre" font-size="38" letter-spacing="0em"><tspan x="399" y="431.488">HTTP request</tspan></text>
<defs>
<filter id="filter0_d_5_61" x="1212.15" y="752.766" width="60.9863" height="13.2324" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
<feFlood flood-opacity="0" result="BackgroundImageFix"></feFlood>
<feColorMatrix in="SourceAlpha" type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0" result="hardAlpha"></feColorMatrix>
<feOffset dy="4"></feOffset>
<feGaussianBlur stdDeviation="2"></feGaussianBlur>
<feComposite in2="hardAlpha" operator="out"></feComposite>
<feColorMatrix type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.25 0"></feColorMatrix>
<feBlend mode="normal" in2="BackgroundImageFix" result="effect1_dropShadow_5_61"></feBlend>
<feBlend mode="normal" in="SourceGraphic" in2="effect1_dropShadow_5_61" result="shape"></feBlend>
</filter>
</defs>
</svg>

But bundle-based dev servers need to **rebundle all the source code** before you can see changes in a browser, because of that some bundle-based dev servers support HMR, but **the time to spin up the dev server increases linearly with bundle size**.

<svg viewBox="0 0 1896 1071" fill="none" xmlns="http://www.w3.org/2000/svg">
<text fill="#FFAA3E" xml:space="preserve" style="white-space: pre" font-size="80" letter-spacing="0em"><tspan x="46" y="132.344">Bundle based dev server</tspan></text>
<rect x="48" y="239" width="1086" height="767" rx="98" stroke="#FFC36B" stroke-width="4"></rect>
<rect x="108" y="577" width="212" height="83" rx="10" fill="#C3E88C"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="170" y="631.488">entry</tspan></text>
<rect x="476" y="712" width="212" height="88" rx="10" fill="#4FC08D"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0.33em"><tspan x="552.5" y="768.988">···</tspan></text>
<rect x="476" y="438" width="212" height="88" rx="10" fill="#4FC08D"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="537" y="494.988">route</tspan></text>
<rect x="473" y="576" width="212" height="88" rx="10" fill="#4FC08D"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="534" y="632.988">route</tspan></text>
<path d="M472.614 481.699L438.815 489.291L462.289 514.766L472.614 481.699ZM324.582 622.18L454.791 502.201L450.726 497.789L320.516 617.768L324.582 622.18Z" fill="#E06666"></path>
<path d="M469 620L439 602.679V637.321L469 620ZM323 623H442V617H323V623Z" fill="#E06666"></path>
<path d="M472.614 756.105L462.032 723.12L438.757 748.777L472.614 756.105ZM320.533 622.196L450.601 740.186L454.632 735.742L324.565 617.752L320.533 622.196Z" fill="#E06666"></path>
<path d="M822.052 905.098L815.036 871.175L789.166 894.213L822.052 905.098ZM689.041 760.243L801.856 886.929L806.337 882.939L693.521 756.253L689.041 760.243Z" fill="#FFC36B"></path>
<path d="M819.908 756.105L811.894 722.403L786.715 746.195L819.908 756.105ZM689.1 622.034L799.185 738.54L803.546 734.419L693.462 617.914L689.1 622.034Z" fill="#FFC36B"></path>
<path d="M817.765 623.19L788.215 605.112L787.334 639.742L817.765 623.19ZM691.205 622.973L790.697 625.502L790.85 619.504L691.357 616.975L691.205 622.973Z" fill="#FFC36B"></path>
<path d="M818.837 481.699L789.286 463.622L788.406 498.252L818.837 481.699ZM692.277 481.483L791.769 484.012L791.922 478.014L692.429 475.485L692.277 481.483Z" fill="#FFC36B"></path>
<path d="M819.909 340.209L786.924 350.795L812.584 374.067L819.909 340.209ZM696.719 480.499L803.992 362.224L799.547 358.193L692.275 476.468L696.719 480.499Z" fill="#FFC36B"></path>
<path d="M817.765 614.614L810.467 580.751L784.789 604.002L817.765 614.614ZM692.273 480.497L797.418 596.614L801.866 592.587L696.721 476.47L692.273 480.497Z" fill="#FFC36B"></path>
<rect x="822" y="288" width="212" height="88" rx="10" fill="#4FC08D"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="864" y="344.988">module</tspan></text>
<rect x="822" y="435" width="212" height="87" rx="10" fill="#4FC08D"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="864" y="491.488">module</tspan></text>
<rect x="820" y="571" width="212" height="88" rx="10" fill="#4FC08D"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="862" y="627.988">module</tspan></text>
<rect x="822" y="718" width="212" height="87" rx="10" fill="#4FC08D"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="864" y="774.488">module</tspan></text>
<rect x="822" y="864" width="212" height="88" rx="10" fill="#4FC08D"></rect>
<text fill="#15505C" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0.33em"><tspan x="898.5" y="920.988">···</tspan></text>
<path d="M1239 627L1209 609.679V644.321L1239 627ZM1136 630H1212V624H1136V630Z" fill="#FFC36B"></path>
<path d="M1596 627L1566 609.679V644.321L1596 627ZM1493 630H1569V624H1493V630Z" fill="#FFC36B"></path>
<rect x="1239" y="545" width="254" height="144" rx="10" fill="#C692EA"></rect>
<text fill="white" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="1306.5" y="629.988">Bundle</tspan></text>
<rect x="1596" y="543" width="254" height="143" rx="10" fill="#009688"></rect>
<text fill="white" xml:space="preserve" style="white-space: pre" font-size="38" font-weight="600" letter-spacing="0em"><tspan x="1667.71" y="604.988">Server
</tspan><tspan x="1675.76" y="649.988">ready</tspan></text>
</svg>

### Bundle for Production

Using the ESM for production increases the num of requests that decrease performance.  
Now still better to bundle code with tree-shaking, lazy-loading and chunk-splitting.

## Babel

**Babel** — is a transcompiler that convert your modern javascript(JSX, etc.) into ECMASript which can be run in a browser.

Babel is the relatively old and reliable toolchain. It also supports a lot of plugins and presets.

Written in JavaScript.

## SWC

**SWC**( Speedy Web Compiler) — is a super-fast JS/TS compiler written in Rust.  
Is isn't support typechecking for TS.  
It takes JavaScript / TypeScript files using modern JavaScript features and outputs valid code that is supported by all major browsers.  
On the [swc.rs](https://swc.rs) you can read that `SWC is 20x faster than Babel` and this is probably true, because SWC is written in Rust, while Babel is written in slow JS.
