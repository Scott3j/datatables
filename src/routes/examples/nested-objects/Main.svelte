<script lang="ts">
    import myData from '$data/test'
    import {
        DataHandler,
        Datatable,
        Th, ThFilter
    } from '$lib/core'

    const handler = new DataHandler(myData, { rowsPerPage: 50 })
    const rows = handler.getRows()

    type Car = { model: string, manufacturer: string }
    type Row = { firstname: string, lastname: string, country: string, car: Car[]}

    const callback = (row: Row) => {
        const arr = row.car.map( (car: Car) => {
            return `${car.model} (${car.manufacturer})`
        })
        return arr.join(', ')
    }
</script>


<Datatable {handler}>
    <table>
        <thead>
            <tr>
                <Th {handler} orderBy="firstname">First Name</Th>
                <Th {handler} orderBy="lastname">Last Name</Th>
                <Th {handler} orderBy="country">Country</Th>
                <Th {handler} orderBy={row => callback(row)}>Car</Th>
            </tr>
            <tr>
                <ThFilter {handler} filterBy="firstname"/>
                <ThFilter {handler} filterBy="lastname"/>
                <ThFilter {handler} filterBy="country"/>
                <ThFilter {handler} filterBy="car"/>
            </tr>
        </thead>
        <tbody>
        {#each $rows as row}
            <tr>
                <td>{row.firstname}</td>
                <td>{row.lastname}</td>
                <td>{@html row.country ?? '<code>null</code>'}</td>
                <td class="list">
                    <ul>
                        {#each row.car as car}
                            <li style:color="{car.hex}">
                                {car.model} ({car.manufacturer})
                            </li>
                        {/each}
                    </ul>
                </td>
            </tr>
        {/each}
        </tbody>
    </table>
</Datatable>



<style>
    thead{
        background:#fff;
    }
    tbody td{
        padding:4px;
    }
    tbody tr:nth-child(even){
        background:#fafafa;
    }
    tbody tr{
        transition:all, 0.2s;
    }
    tbody tr:hover{
        background:#f5f5f5;
    }
    td.list {
        text-align:left;
        white-space: nowrap;
    }
    td ul {
        margin: 0; 
        padding: 0;
        list-style-type: square;
    }
    td li{
        font-weight: bold;
        font-size: 12px;
    }
    td :global(code) {
        color: #bdbdbd;
        font-size:12px;
    }
</style>