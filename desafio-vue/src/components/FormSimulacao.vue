<template>
    <div class="min-height row">
        <div class="col s12">
            <header id="title-page">
                <p>Faça sua simulação de empréstimo</p>
            </header>
            <form class="card-panel" @submit="makeSimulation($event)">
                <!--Input de emprestimo-->
                <div class="row">
                    <div class="col m10 offset-m1">
                         <div class="input-title">
                            <p>Valor do empréstimo (R$)</p>
                         </div>
                        <div class="input-field">
                            <input 
                            placeholder="0.00"
                            required
                            id="loan-amount"
                            type="number"
                            step="any"
                            class="validate"
                            @keyup="validateLoanValue($event)"
                            @change="fixValue($event)">
                            
                        </div>
                    </div>
                </div>
                <!--Input de Instituição-->
                <div class="row">
                    <div class="col s12 m10 offset-m1">
                        <div class="input-title">
                        <p>Selecione as instituições desejadas:</p>
                         </div>
                        <p v-for="instituition in institutions" :key="instituition.chave">
                            <label>
                                <input type="checkbox" v-model="institutionsSelected" :value="instituition.chave" name="institutions"/>
                                <span>{{instituition.valor}}</span>
                            </label>
                        </p>
                    </div>
                </div>
                <!--Input de Convenio-->
                <div class="row">
                    <div class="col s12 m10 offset-m1">
                        <div class="input-title">
                            <p>Selecione os convênios desejados:</p>
                        </div>
                        <p v-for="contract in contracts" :key="contract.chave">
                            <label>
                                <input type="checkbox" v-model="contractsSelected" :value="contract.chave" name="contracts"/>
                                <span>{{contract.valor}}</span>
                            </label>
                        </p>
                    </div>
                </div>

                <!--Input de Parcelas-->
                <div class="row">
                    <div class="col s12 m10 offset-m1">
                        <div class="input-field">
                            <select v-model="installment">
                                <option :value="installment">Selecione o número de Parcelas</option>
                                <option v-for="(installment, index) in installments" 
                                :key="index" :value="installment">{{installment}}x</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!--Botão simulação-->
                <div class="row">
                    <div class="col s4 offset-s4 m4 offset-m8 l4 offset-l8 xl1 offset-xl9">
                        <button class="btn waves-effect waves-light" type="submit" name="action">Simulação
                            <i class="material-icons right">send</i>
                        </button>
                    </div>
                </div>
            </form>
        </div>
    </div>
</template>
<script>
export default {
    name: 'FormSimulacao',
    methods: {
        validateLoanValue($event) {
            let field = $event.target
            let value = Number.parseFloat((field.value))

            if(isNaN(value) && field.value.length == 0) {
                this.loanValue = 0
                return field.value = ''
            }
            this.loanValue = this.withTwoDecimals(value)
        },
        fixValue($event) {
            //caso na posição dos decimais nao tenham 2 algarismos adiciona um ou dois zeros a direita
            let numStringFormat = this.loanValue + ''
            let numberArr = numStringFormat.split('.')

            if( numberArr.length < 2) {
                numStringFormat += '.00'
            } else {
                if (numberArr[1].length < 2) {
                    numStringFormat += '0'
                }
            }
            $event.target.value = numStringFormat
        },
        withTwoDecimals(number) {
            const DECIMAL_PLACES = 2
            let aux = Math.pow(10, DECIMAL_PLACES)
            return Math.floor(number * aux) / aux;
        },
        BRLFormatCurrency(value) {
            return value.toLocaleString('pt-br',{style: 'currency', currency: 'BRL'})
            
        },
        async getInstitutions() {
            try {
                let req = await fetch(`${this.apiURL}instituicao`)

                let data = await req.json();
                this.institutions = data;
            } catch (e) {
                console.error(`Erro getInstituicao: ${e}`);
            }
        },
        async getContracts() {
            try {
                let req = await fetch(`${this.apiURL}convenio`)
                let data = await req.json();
                this.contracts = data;
            } catch (e) {
                console.error(`Erro getContracts: ${e}`);
            }
        },
        async makeSimulation($event) {
            $event.preventDefault()

            let arrContracts = Array.from(this.contractsSelected)
            let arrInstitutions = Array.from(this.institutionsSelected)
            let data = {
                "valor_emprestimo": this.loanValue,
                "instituicoes" : arrInstitutions,
                "convenios" : arrContracts,
                "parcelas": this.installment == ''? 0: this.installment
                
            }
            data = JSON.stringify(data)

            const req = await fetch(`${this.apiURL}simular`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json'},
                body: data
            })

            const res = await req.json();
            
            this.$emit('makedSimulation', res)
        },
        
    },
    data() {
        return {
            apiURL:'http://127.0.0.1:8000/api/',
            loanValue: 0,
            installments: [36, 48, 60, 72, 84],
            installment: "",
            institutions: null,
            institutionsSelected: [],
            contracts: null,
            contractsSelected: []
        }
    },
    mounted() {
        this.getInstitutions()
        this.getContracts()
    }
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
    input[type=number]::-webkit-inner-spin-button { 
       -webkit-appearance: none;
        
    }
    input[type=number] { 
        -moz-appearance: textfield;
        appearance: textfield;
    }
    .input-title {
        font-weight: bold;
        padding-left: 8px;
        border-left: solid 4px #0277bd;
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