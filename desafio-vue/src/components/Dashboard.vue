<template>
    <div class="min-height row">
        <div class="col s12">
            <header id="title-page">
                <p>Resultados da simulação</p>
            </header>
            <div class="card-panel">
                <table>
                    <thead>
                    <tr>
                        <th>Instituição</th>
                        <th>Valor Solicitado</th>
                        <th>Nº parcelas x Valor</th>
                        <th>Taxa de juros a.m</th>
                    </tr>
                    </thead>

                    <tbody>
                    <tr>
                        <td>Alvin</td>
                        <td>Eclair</td>
                        <td>$0.87</td>
                    </tr>
                    </tbody>
                </table>
            </div>

            <div class="row">
                <div class="col s4 offset-s4 m4 offset-m8 l4 offset-l8 xl1 offset-xl9">
                    <button class="btn waves-effect waves-light" @click="$emit('closeDashboard')">Fechar
                        <i class="material-icons right">close</i>
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Dashboard',
    props: ['dataSimulation'],
    
    
}
</script>
<style scoped>
#title-page {
        font-size: 32px;
        font-weight: bold;
    }
    @media (max-width: 700px) {
        #title-page {
        font-size: 18px;
        }
    }

    .min-height {
        min-height: 550px;
    }
    .card-panel {
        padding-top: 30px;
    }
    

    .btn {
        background-color: #0277bd;
        width: 150px;
        transition: .5s;
    }

    .btn:hover {
        background-color: #b3e5fc ;
        color: #222;
        width: 150px;
    }

    label span {
        color: #000;
    }
</style>