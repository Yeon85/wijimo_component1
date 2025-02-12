<template>
    <div class="container">
        <Menu :widgets="widgets" @add-tile="addTile" />
        <div class="hr"></div>
        <Dashboard :tiles="tiles"
                   @remove-tile="removeTile"
                   @drag-start="dragStart"
                   @drag-over="dragOver"
                   @drag-finish="dragFinish"
                   @drag-end="dragEnd"
        />
    </div>
</template>

<script>
    import Menu from './components/Menu.vue';
    import Dashboard from './components/Dashboard.vue';
    import { widgets, useTiles } from './tiles';

    export default {
        name: 'app',
        components: {
            Menu,
            Dashboard,
        },
        setup() {
            const { tiles, addTile, ...other } = useTiles(true);

            // add initial tiles
            addTile(widgets[0].id); // Grid
            addTile(widgets[7].id); // Bullet Graph
            addTile(widgets[5].id); // Line Chart
            addTile(widgets[2].id); // Linear Gauge
            addTile(widgets[1].id); // Radial Gauge

            return {
                widgets,
                tiles,
                addTile,
                ...other,
            }
        },
    };
</script>

<style>
    /* Wijmo and Material Design Lite */
    @import '../node_modules/@mescius/wijmo.styles/themes/material/wijmo.theme.material.indigo-amber.css';

    *, *::before, *::after {
        box-sizing: border-box;
    }

    /* customize the browser's scrollbar: */
    *::-webkit-scrollbar {
        width: 6px;
        height: 6px;
    }
    *::-webkit-scrollbar-track {
        border-radius: 0.25rem;
        background: rgba(0, 0, 0, 0.1);
    }
    *::-webkit-scrollbar-thumb {
        border-radius: 0.25rem;
        background: rgba(0, 0, 0, 0.2);
    }
    *::-webkit-scrollbar-thumb:hover {
        background: rgba(0, 0, 0, 0.4);
    }
    *::-webkit-scrollbar-thumb:active {
        background: rgba(0, 0, 0, 0.9);
    }

    html,
    body {
        height: 100%;
    }
    body {
        background-color: #f7faff;
        font-size: 0.875rem;
        font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Ubuntu, 'Helvetica Neue', sans-serif;
        padding: 0;
        margin: 0;
        display: flex;
        flex-direction: column;
    }

    .button {
        cursor: pointer;
    }

    #app {
        flex: 1 1 auto;
        overflow: hidden;
    }

    .container {
        display: flex;
        flex-direction: row;
        height: 100%;
        background: #f2f6fe;
    }

    .hr {
        width: 1px;
        height: 100%;
        position: relative;
        background: #e4ecfb;
        z-index: 1;
    }
    .hr::before {
        content: '';
        display: block;
        width: 1px;
        height: 100%;
        background-color: #f6f9fe;
    }

    .menu {
        display: flex;
        flex-direction: column;
        font-size: 0.85rem;
        justify-content: flex-start;
        flex: 0 0 auto;
        padding: 0;
    }
    @media only screen and (max-width: 840px), (max-height: 840px) {
        .menu {
            overflow: auto;
            font: 0.75rem;
        }
    }
    .menu.menu--open .menu-item-name {
        display: block;
    }

    .menu-toggle {
        transition: all 500ms;
        padding: 1rem;
        text-align: center;
        border-bottom: 1px solid #e6ecf1;
        cursor: pointer;
    }

    .menu-item {
        display: flex;
        flex-direction: row;
        align-items: center;
        padding: 0.5rem 1rem;
        cursor: pointer;
        user-select: none;
        position: relative;
        text-align: left;
    }
    .menu-item:hover {
        background: #f7faff;
    }
    .menu-item:hover:before {
        content: '';
        display: inline-block;
        width: 1rem;
        height: 1rem;
        bottom: 2rem;
        left: 2.5rem;
        position: absolute;
        background: linear-gradient(#fff, #fff), linear-gradient(#fff, #fff), #0085c7;
        background-position: center;
        background-size: 50% 2px, 2px 50%; /*thickness = 2px, length = 50% (25px)*/
        background-repeat: no-repeat;
        border-radius: 50%;
        box-shadow: 0 1px 2px rgba(55, 63, 66, 0.07), 0 2px 4px rgba(55, 63, 66, 0.07), 0 4px 8px rgba(55, 63, 66, 0.07),
        0 8px 16px rgba(55, 63, 66, 0.07), 0 16px 24px rgba(55, 63, 66, 0.07), 0 24px 32px rgba(55, 63, 66, 0.07);
    }
    .menu-item-name {
        margin: 0 0.5rem 0 1rem;
        white-space: nowrap;
        display: none;
    }
    @media only screen and (max-width: 840px), (max-height: 840px) {
        .menu-toggle {
            padding: 1rem 0;
        }
        .menu-item {
            padding: 0.5rem;
        }
        .menu-item:hover:before {
            bottom: 1rem;
            left: 1rem;
        }
        .menu-item svg {
            width: 32px;
            height: 32px;
        }
    }

    .menu .menu-toggle svg {
        transition: all 250ms ease-out 50ms;
        transform-origin: center center;
        transform: scaleX(1);
    }
    .menu.menu--open .menu-toggle svg {
        transform: scaleX(-1);
    }

    .content {
        flex: 1 1 auto;
        overflow: auto;
        background-color: #f7faff;
        width: 100%;
        padding: 2rem;
        box-sizing: border-box;
    }
    @media only screen and (max-width: 811px) {
        .content {
            padding: 0.25rem;
        }
    }
    .dashboard {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        margin: auto;
        height: 100%;
    }
    .blank {
        display: flex;
        flex-direction: column;
        align-items: center;
        width: 100%;
        height: 100%;
        justify-content: center;
        text-align: center;
    }
    .blank svg {
        width: 3rem;
        height: 3rem;
        margin-bottom: 1rem;
    }

    .tile {
        display: flex;
        flex-direction: column;
        flex: 0 0 auto;
        width: calc(25% - 1rem);
        margin: 0.5rem;
        height: 50vh;
        max-height: calc(50% - 1rem);
        overflow: hidden;
        background: white;
        page-break-inside: avoid; /* important when printing the dashboard */
        transition: all 250ms;
        border-radius: 0.5rem;
        box-sizing: border-box;
        box-shadow: 0 1px 2px rgba(55, 63, 66, 0.07), 0 2px 4px rgba(55, 63, 66, 0.07), 0 4px 8px rgba(55, 63, 66, 0.07),
        0 8px 16px rgba(55, 63, 66, 0.07), 0 16px 24px rgba(55, 63, 66, 0.07), 0 24px 32px rgba(55, 63, 66, 0.07);
    }
    @media only screen and (max-width: 1599px) {
        .tile {
            width: calc(33.33% - 1rem);
        }
    }
    @media only screen and (max-width: 1079px) {
        .tile {
            width: calc(50% - 1rem);
        }
    }
    @media only screen and (max-width: 1023px) {
        .tile {
            width: calc(100% - 1rem);
        }
    }
    @media only screen and (max-height: 800px) {
        .tile {
            max-height: 400px;
        }
    }
    .tile:last-child {
        flex: 1 1 auto;
    }
    .tile:hover {
        border-color: #adb7bd;
    }
    .tile.drag-over {
        border: 2px dashed #000;
    }
    .tile .buttons {
        transition: all 250ms;
        opacity: 0;
    }
    @media (hover: none) and (pointer: coarse) {
        .tile .buttons {
            opacity: 1;
        }
    }
    .tile:hover .buttons {
        opacity: 1;
    }
    .tile .buttons > span {
        padding: 0 0.5rem;
        cursor: pointer;
    }
    .tile.drag-over {
        border: 2px dashed #000;
        background-color: rgba(0, 0, 0, 0.1);
        transition: all 250ms;
    }
    .tile.drag-source {
        opacity: 0.4;
        box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
        background-color: rgba(145, 200, 248, 0.75);
        transform: scale(0.9);
        transition: all 250ms;
    }
    .tile .tile-container {
        border-bottom: 1px solid #e0e0e0;
        padding: 0.75rem 1rem;
        display: flex;
        cursor: move;
    }
    .tile .tile-header {
        flex-grow: 1;
        font-size: 1rem;
        font-weight: 400;
        padding: 0.125rem;
        opacity: 0.75;
    }
    .tile .tile-content {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        flex: 1 1 auto;
        overflow: auto;
        height: 100%;
    }
    .tile .blank-tile {
        height: 100vh;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .wj-radialgauge {
        width: 60%;
        height: 60%;
        max-width: 300px;
        padding: 1rem;
        overflow: hidden;
    }
    .wj-radialgauge .wj-value {
        font-size: 0.75rem;
        font-weight: 500;
    }
    .wj-gauge .wj-face path {
        stroke: none;
    }

    .wj-flexchart {
        background: transparent;
        height: calc(100%);
        width: 100%;
        border: none;
        padding: 1rem;
        margin: 0;
        overflow: hidden;
    }
    .wj-ranges {
        opacity: 0.15;
    }

    h3 {
        font-size: 0.875rem;
        font-weight: 500;
        text-align: center;
    }
    h4 {
        font-size: 0.875rem;
        font-size: 0.75rem;
        font-weight: 400;
        min-width: 7rem;
    }
    .wj-lineargauge {
        max-height: 1rem;
        width: 100%;
        overflow: hidden;
    }
    .line-gauge-tile {
        width: 100%;
        padding: 0 1rem;
    }
    .flex-row {
        display: flex;
        flex-direction: row;
        align-items: center;
        padding: 0 1rem;
    }
    .wj-ranges {
        opacity: 0.15;
    }

    .wj-flexgrid {
        border: none;
        height: 100%;
    }
    .wj-flexgrid .wj-cell {
        border-right: none;
        border-bottom: 1px solid rgba(0, 0, 0, 0.1);
        padding: 0.35rem 1rem;
        font-size: 0.8125rem;
    }

    .bulet-graph-tile {
        width: 100%;
        padding: 0 1rem;
        overflow: hidden;
    }
    .table {
        margin: 0;
        table-layout: fixed;
        width: 100%;
    }
    .table td {
        padding: 0.15rem 0.5rem;
        font-size: 0.75rem;
        white-space: nowrap;
        width: 1.5rem;
    }
    .table td:first-child {
        width: 4rem;
    }
    .table td:last-child {
        width: auto;
    }
</style>
