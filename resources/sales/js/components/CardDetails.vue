<!--
  - CatLab Drinks - Simple bar automation system
  - Copyright (C) 2019 Thijs Van der Schaeghe
  - CatLab Interactive bvba, Gent, Belgium
  - http://www.catlab.eu/
  -
  - This program is free software; you can redistribute it and/or modify
  - it under the terms of the GNU General Public License as published by
  - the Free Software Foundation; either version 3 of the License, or
  - (at your option) any later version.
  -
  - This program is distributed in the hope that it will be useful,
  - but WITHOUT ANY WARRANTY; without even the implied warranty of
  - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  - GNU General Public License for more details.
  -
  - You should have received a copy of the GNU General Public License along
  - with this program; if not, write to the Free Software Foundation, Inc.,
  - 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
  -->

<template>

    <div>
        <div v-if="loading"><b-spinner /></div>
        <div v-if="card">
            <table class="table">

                <tr>
                    <td>ID</td>
                    <td>{{ card.id }}</td>
                </tr>

                <tr v-if="card.name">
                    <td>Name</td>
                    <td>{{ card.name }}</td>
                </tr>

                <tr>
                    <td>Balance</td>
                    <td>{{ VisibleAmount.toVisible(card.balance) }}</td>
                </tr>

                <tr v-if="card.discount !== 0">
                    <td>Discount</td>
                    <td>{{ card.discount }}</td>
                </tr>

            </table>

            <h3>Topup</h3>
            <card-topup :card="card" />

            <h3>Transactions</h3>
            <transactions-table :cardId="card.id" />
        </div>
    </div>

</template>

<script>

    import {MenuService} from "../services/MenuService";
    import {OrderService} from "../services/OrderService";
    import {VisibleAmount} from "../nfccards/tools/VisibleAmount";

    export default {

        props: [
            'cardUid'
        ],

        mounted() {
            this.loading = true;
            this.card = null;
            this.loadCardData();
        },

        watch: {

        },

        data() {
            return {
                card: null,
                loading: false,
                VisibleAmount: VisibleAmount
            }
        },

        methods: {

            async loadCardData() {
                this.card = await this.$cardService.getCardFromUid(this.cardUid);
                this.loading = false;
            },

            topup() {
                alert('Topping up');
            }

        }
    }
</script>
