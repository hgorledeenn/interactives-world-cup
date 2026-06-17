<script>
    import {scaleSqrt, scaleLinear} from 'd3';

    const trophies = [
        {name: "FIFA World Cup",         inches: 14.5, worth: 240000, icon: "/icons/fifa_world_cup.svg",          ar: 282.97/715.72},
        {name: "FIFA Women's World Cup", inches: 19,   worth: 30000,  icon: "/icons/fifa_world_cup_women.svg",    ar: 166.83/627.92},
        {name: "English Premier League", inches: 41,   worth: 5500,   icon: "/icons/premier_league_trophy.svg",  ar: 316.24/596.36},
        {name: "Henri Delaunay Cup",     inches: 24,   worth: 150000, icon: "/icons/henri_delaunay.svg",          ar: 254.75/636.37},
        {name: "Italian Champion's Cup", inches: 22.8, worth: 66000,  icon: "/icons/italian_champions_cup.svg",  ar: 341.14/512.14},
    ];

    const W = 1400, H = 750;
    const ml = 130, mr = 50, mt = 140, mb = 80;
    const pw = W - ml - mr;
    const ph = H - mt - mb;

    const xMax = 250000;
    const yMax = 45;
    const xTicks = [10000, 50000, 100000, 150000, 200000, 250000];
    const yTicks = [10, 20, 30, 40];

    const xs = scaleSqrt().domain([0, xMax]).range([ml, ml + pw]);
    const ys = scaleLinear().domain([0, yMax]).range([mt + ph, mt]);
    function fmtWorth(v) { return '$' + (v / 1000) + 'k'; }
</script>

<svg width={W} height={H} style="background:white; display:block; margin: 40px auto;">

    <!-- Title -->
    <text x={ml} y={30} font-size="18" font-weight="bold" font-family="Helvetica" fill="black">
        <tspan fill="black">The </tspan> <tspan fill="green">FIFA</tspan> <tspan fill="blue">World</tspan> <tspan fill="red">Cup</tspan> <tspan fill="black"> trophy won't take up much shelf space, but it sure is expensive</tspan>
    </text>
    <text x={ml} y={55} font-size="14" font-family="Helvetica" fill="black">Although the FIFA World Cup Trophy is one of the smallest major trophies in soccer by height, it is by far the most valuable,</text>
    <text x={ml} y={72} font-size="14" font-family="Helvetica" fill="black">with an estimated worth of approximately $240,000 USD. The next most valuable, the Henri Delaunay Cup, is valued at ~$150,000 USD.</text>

    <!-- Grid lines -->
    {#each yTicks as t}
        <line x1={ml} y1={ys(t)} x2={ml+pw} y2={ys(t)} stroke="#ccc" stroke-width="1" stroke-dasharray="4 4"/>
    {/each}

    <!-- X axis -->
    <line x1={ml} y1={mt+ph} x2={ml+pw+16} y2={mt+ph} stroke="#111" stroke-width="2"/>
    <polygon points="{ml+pw+16},{mt+ph-5} {ml+pw+26},{mt+ph} {ml+pw+16},{mt+ph+5}" fill="#111"/>

    <!-- Y axis -->
    <line x1={ml} y1={mt+ph} x2={ml} y2={mt-16} stroke="#111" stroke-width="2"/>
    <polygon points="{ml-5},{mt-16} {ml},{mt-26} {ml+5},{mt-16}" fill="#111"/>

    <!-- Worth labels -->
    {#each xTicks as t}
        <text x={xs(t)} y={mt+ph+22} text-anchor="middle" font-size="13" font-family="Helvetica" fill="#111">{fmtWorth(t)}
        </text>
    {/each}

    <!-- X axis label -->
    <text x={ml + pw/2} y={mt+ph+50} text-anchor="middle" font-size="14" font-family="Helvetica" fill="#111">Worth (USD)
    </text>

    <!-- Height labels -->
    {#each yTicks as t}
        <text x={ml-10} y={ys(t)+4} text-anchor="end" font-size="13" font-family="Helvetica" fill="#111">
            {t} in
        </text>
    {/each}

    <!-- Y axis label -->
    <text
        x={-(mt + ph/2)}
        y={22}
        text-anchor="middle"
        font-size="14"
        font-family="Helvetica"
        fill="#111"
        transform="rotate(-90)"
    >Height (inches)</text>

    <!-- Trophy icons + labels -->
    {#each trophies as t}
        {@const h = ys(0) - ys(t.inches)}
        {@const w = h * t.ar}
        <image
            href={t.icon}
            x={xs(t.worth) - w / 2}
            y={ys(0) - h}
            width={w}
            height={h}
        />
        {#if t.name === "FIFA World Cup"}
            <text x={xs(t.worth)} y={ys(t.inches) - 8} font-size="12" font-family="Helvetica" text-anchor="middle">
                <tspan fill="green">FIFA</tspan>
                <tspan fill="blue"> World</tspan>
                <tspan fill="red"> Cup</tspan>
            </text>
        {:else}
            <text x={xs(t.worth)} y={ys(t.inches) - 8} font-size="12" font-family="Helvetica" fill="#333" text-anchor="middle">
                {t.name}
            </text>
        {/if}
    {/each}

</svg>

<style>
	.story-card {
		margin-bottom: 3rem;
	}

	.divider {
		border: none;
		border-top: 1px solid #222;
		margin: 0 0 2rem;
	}

	.headline {
		font-size: 1.75rem;
		font-weight: 700;
		line-height: 1.2;
		margin: 0 0 0.5rem;
	}

	.byline {
		font-size: 0.875rem;
		color: #666;
		margin: 0 0 1.5rem;
	}

	p {
		font-size: 1.0625rem;
		line-height: 1.7;
		margin: 0 0 1rem;
	}

	.chart {
		margin: 2rem 0 0;
	}

	.chart-title {
		font-size: 0.875rem;
		font-weight: 600;
		margin-bottom: 0.75rem;
	}

	.chart-workspace {
		display: flex;
		align-items: center;
		justify-content: center;
		min-height: 320px;
		border: 1px solid #ddd;
		background: #fafafa;
	}

	.chart-placeholder {
		margin: 0;
		font-size: 0.875rem;
		color: #999;
	}

	.chart-footer {
		margin-top: 0.75rem;
		font-size: 0.8125rem;
		color: #666;
	}
</style>
