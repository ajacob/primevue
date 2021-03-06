<template>
	<div>
		<div class="content-section introduction">
			<div class="feature-intro">
				<h1>DataView</h1>
				<p>DataView displays data in grid or list layout with pagination and sorting features.</p>
			</div>
		</div>

		<div class="content-section implementation">
			<DataView :value="cars" :layout="layout" paginatorPosition="both" :paginator="true" :rows="20" :sortOrder="sortOrder" :sortField="sortField">
				<template #header>
					<div class="p-grid p-nogutter">
						<div class="p-col-6" style="text-align: left">
							<Dropdown v-model="sortKey" :options="sortOptions" optionLabel="label" placeholder="Sort By" @change="onSortChange($event)"/>
						</div>
						<div class="p-col-6" style="text-align: right">
							<DataViewLayoutOptions v-model="layout" />
						</div>
					</div>
				</template>
				<template #list="slotProps" >
					<div class="p-col-12">
                        <div class="car-details">
                            <div>
                                <img :src="'demo/images/car/' + slotProps.data.brand + '.png'" :alt="slotProps.data.brand"/>
                                <div class="p-grid">
                                    <div class="p-col-12">Vin: <b>{{slotProps.data.vin}}</b></div>
                                    <div class="p-col-12">Year: <b>{{slotProps.data.year}}</b></div>
                                    <div class="p-col-12">Brand: <b>{{slotProps.data.brand}}</b></div>
                                    <div class="p-col-12">Color: <b>{{slotProps.data.color}}</b></div>
                                </div>
                            </div>
                            <Button icon="pi pi-search"></Button>
                        </div>
                    </div>
				</template>
				<template #grid="slotProps">
					<div style="padding: .5em" class="p-col-12 p-md-3">
						<Panel :header="slotProps.data.vin" style="text-align: center">
							<img :src="'demo/images/car/' + slotProps.data.brand + '.png'" :alt="slotProps.data.brand"/>
							<div class="car-detail">{{slotProps.data.year}} - {{slotProps.data.color}}</div>
							<Button icon="pi pi-search"></Button>
						</Panel>
					</div>
				</template>
			</DataView>
		</div>

		<DataViewDoc />
	</div>
</template>

<script>
import CarService from '../../service/CarService';
import DataViewDoc from './DataViewDoc';

export default {
    data() {
        return {
            cars: null,
            layout: 'list',
            sortKey: null,
            sortOrder: null,
            sortField: null,
            sortOptions: [
                {label: 'Newest First', value: '!year'},
                {label: 'Oldest First', value: 'year'},
                {label: 'Brand', value: 'brand'}
            ]
        }
    },
    carService: null,
    created() {
        this.carService = new CarService();
    },
    mounted() {
        this.carService.getCarsLarge().then(data => this.cars = data);
    },
    methods: {
        onSortChange(event){
            const value = event.value.value;
            const sortValue = event.value;

            if (value.indexOf('!') === 0) {
                this.sortOrder = -1;
                this.sortField = value.substring(1, value.length);
                this.sortKey = sortValue;
            }
            else {
                this.sortOrder = 1;
                this.sortField = value;
                this.sortKey = sortValue;
            }
        }
    },
    components: {
        'DataViewDoc': DataViewDoc
    }
}
</script>

<style lang="scss" scoped>
.p-dropdown {
    width: 12em;
    font-weight: normal;
}

.p-dataview {
    .car-details {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 2em;

        & > div {
            display: flex;
            align-items: center;

            img {
                margin-right: 14px;
            }
        }
    }

    .car-detail {
        padding: 0 1em 1em 1em;
        border-bottom: 1px solid #d9dad9;
        margin: 1em;
    }

    .p-panel-content {
        padding: 1em;
    }
}

@media (max-width: 1024px) {
	.p-dataview {
        .car-details {
            img {
                width: 75px;
            }
        }
    }
}

/* Dark Theme such as luna-amber, luna-blue, luna-green and luna-pink */
.dark-theme {
    .p-dataview {
        .car-detail {
            border-bottom: 1px solid #191919;
        }
    }
}
</style>