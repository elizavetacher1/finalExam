<template>
    <div class="filterMenu"> 
        <b-navbar class="dropdown-menu">
            <sortByYear
                class="dropdown-item"
                :filter="filterBy"
                :options="options"
                @select="optionSelect"
                @removePopMov="removePopMov" 
                @backPop="backPop"
                @setfilter="filterBy = $event    
                $emit('setfilter', 'year')"         
            >
            </sortByYear>
            <genresButtons 
                :filter="filterBy"
                :allPopular="allPopular"
                @removePopMov="removePopMov" 
                @backPop="backPop" 
                @setfilter="filterBy = $event"
                class="dropdown-item"
            >
            </genresButtons>
            <sortByRate 
                :filter="filterBy"
                @removePopMov="removePopMov" 
                @backPop="backPop"
                @setfilter="filterBy = $event"
                class="dropdown-item"
            >
            </sortByRate>
        </b-navbar>
        <img class="logoMovie" src='@/assets/tmdb.png'> 
    </div>
</template>

<script>

import sortByRate from '@/components/blocks/sortByRate'
import genresButtons from '@/components/blocks/genresButtons'
import sortByYear from '@/components/blocks/sortByYear'

export default {
    name: 'filterMenu',
    components: {
        genresButtons,
        sortByYear,
        sortByRate
    },
    props: ['allPopularMovies', 'options', 'filter'],
    methods: {
        optionSelect (option) {
            this.selected = option.id
            this.no = option.abba
        }, 
        reset () {
            this.$emit('reset')
        },
        removePopMov () {
            this.$emit('removePopMov')
        },
        backPop () {
            this.$emit('backPop')
        },
    },
    data: function () {
        return {
            selected: '',
            upForYear: false,
            filterBy: ''
        }
    }
}

</script>

<style lang="scss" scoped>

.logoMovie{
    width: 700px;
    box-shadow: none;
    border-radius: 0;
    position: absolute;
    top: -242px;
    height: 200px;
    left: 653px;
    opacity: 0.1;
    background: #0d221e;    
    border-radius: 38px;
}

    .logoMovie {
        @media (max-width: 1500px) {
            top: 84px;
            left: 7px;
        }
        @media (max-width: 900px) {
            width: 70vw; 
            height: 100px;
        }
}

.filterMenu nav {
   --bs-dropdown-bg: none;
    display: flex;
    justify-content: space-evenly;
    border: none;
    position: absolute;
    top: 1068px;
    font-family: Georgia, serif;
    z-index: 1;
    left: 0;
    @media (max-width: 1500px) {
        top: 1383px;
        left: -711px;
    }
}

.filterMenu {
    position: relative;
}

.navbar-nav {
    margin: 0 auto;
}

.dropdown-toggle::after {
    vertical-align: 0 !important;
    top: 70% !important;
    
}

.navbar {
    .dropdown-item {
        width: 200px;
        white-space: break-spaces !important;
        position: absolute;
        color: white !important;
            &:nth-child(1) {
                top: -1417px;
                left: 691px;
                    @media (max-width: 900px) {
                        top: -1309px;
                        left: 655px;
                }
            }
            &:nth-child(2) {                       
               top: -1417px;
                left: 968px;
                    @media (max-width: 900px) {
                        top: -1309px;
                        left: 764px;
                }
            }
            &:nth-child(3) {
                top: -1417px;
                left: 1145px;
                    @media (max-width: 900px) {
                        top: -1309px;
                        left: 847px;
                }
            }
    }   
}

</style>

<style lang="scss">

.absolute {
    width: 1500px;
    margin: 0 auto;
    padding: 0 35px;
        @media (max-width: 1500px) {
            width: 100vw;
        }
}

#dropdown-1__BV_toggle_ {
    font-size: 25px; 
    padding: 15px 17px 25px;
    background: unset;
    border: none;
    color: #301818;
    text-shadow: 7px 5px 6px #6c936c;
    @media (max-width: 900px) {
        font-size: 15px; 
    }
}

.margin {
    margin-top: 190px;
}


</style>