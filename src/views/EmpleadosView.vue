<template>
    <div class="about">
        <h5 class="mb-5 mt-5">EMPLEADOS Y SUS vehiculos</h5>

        <div class="row">
            <div class="mb-4 col-lg-4">
                <div class="input-group">
                    <span class="input-group-text" id="type-filter"><i class="bi bi-filter "></i> Filtrar por Tipo de
                        vehículo</span>
                    <select class="form-select" @change="toFilter = $event.target.value">
                        <option value="">todos</option>
                        <option v-for="type in types" :value="type" :key="type">{{ type }}</option>
                    </select>
                </div>
            </div>

            <div class="mb-6 col-lg-6">
                <div class="input-group">
                    <span class="input-group-text" id="description-search-text"><i class="bi bi-search"></i> Buscar
                        nombre de empleado </span>
                    <input type="search" class="form-control" id="description-search"
                        placeholder="Escrina y presione Enter" @search="this.toSearch = $event.target.value">
                </div>
            </div>
        </div>

        <div class="card">
            <div class="card-body">
                <form @submit.prevent="save()">
                    <table class="table table-striped">
                        <thead>
                            <tr>
                                <th colspan="2">
                                    <input type="text" class="form-control" id="empleado" v-model="new_auto.empleado"
                                        placeholder="Nuevo empleado">
                                </th>
                                <th>
                                    <select class="form-select" aria-label="Nueva area" v-model="new_auto.area">
                                        <option value="" hidden>Seleccionar</option>
                                        <option v-for="area in areas" :value="area" :key="area">{{ area }}</option>
                                    </select>
                                </th>
                                <th>
                                    <select class="form-select" aria-label="Nuevo tipo de auto" v-model="new_auto.type">
                                        <option value="" hidden>Seleccionar</option>
                                        <option v-for="type in types" :value="type" :key="type">{{ type }}</option>
                                    </select>
                                </th>
                                <th>
                                    <input type="text" class="form-control" id="placa" v-model="new_auto.placa"
                                        placeholder="Nueva placa">
                                </th>
                                <th>
                                    <input type="text" class="form-control" id="color" v-model="new_auto.color"
                                        placeholder="Nuevo color">
                                </th>
                                <th>
                                    <button type="submit" class="btn btn-primary"
                                        title="Agregar nuevo vehiculo">Agregar</button>
                                </th>
                            </tr>
                            <tr>
                                <th scope="col">#</th>
                                <th scope="col">Empleado</th>
                                <th scope="col">Area</th>
                                <th scope="col">Tipo de Vehiculo</th>
                                <th scope="col">Placa</th>
                                <th scope="col">Color</th>
                                <th></th>
                            </tr>
                        </thead>

                        <tbody>
                            <tr v-for="(auto, index) in getList()" :key="index">
                                <th scope="row">{{ 1 + index }}</th>
                                <td>{{ auto.empleado }}</td>
                                <td>{{ auto.area }}</td>
                                <td>{{ auto.type }}</td>
                                <td>{{ auto.placa }}</td>
                                <td>{{ auto.color }}</td>
                                <td>
                                    <button type="button" class="btn btn-warning btn-sm" @click="edit(index)"><i
                                            class="bi bi-pen"></i></button>

                                    <button type="button" class="btn btn-danger btn-sm" @click="remove(index)"><i
                                            class="bi bi-trash3"></i></button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </form>
            </div>
        </div>

        <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="editModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h1 class="modal-title fs-5" id="editModalLabel">Editar Datos</h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <form @submit.prevent="saveEdit()">
                        <div class="modal-body" v-if="itemSelected">
                            <div class="row">

                                <h4>Datos Personales</h4>
                                <div class="mb-8 col-lg-8">
                                    <label for="updateEmpleado" class="form-label">Empleado</label>
                                    <input type="text" v-model="itemSelected.empleado" class="form-control"
                                        id="updateEmpleado">
                                </div>
                                <div class="mb-4 col-lg-4">
                                    <label for="updateArea" class="form-label">Area</label>
                                    <select class="form-select" v-model="itemSelected.area" id="updateArea">
                                        <option v-for="area in areas" :value="area">{{ area }}</option>
                                    </select>
                                </div>
                            </div>
                            <div class="row">
                                <h4>Datos del Vehiculo</h4>
                                <div class="mb-4 col-lg-4">
                                    <label for="updateType" class="form-label">Tipo de Vehiculo</label>
                                    <select class="form-select" v-model="itemSelected.type" id="updateType">
                                        <option v-for="type in types" :value="type">{{ type }}</option>
                                    </select>
                                </div>
                                <div class="mb-4 col-lg-4">
                                    <label for="updatePlaca" class="form-label">Placa</label>
                                    <input type="text" v-model="itemSelected.placa" class="form-control"
                                        id="updatePlaca">
                                </div>
                                <div class="mb-4 col-lg-4">
                                    <label for="updateColor" class="form-label">Color</label>
                                    <input type="text" v-model="itemSelected.color" class="form-control"
                                        id="updateColor">
                                </div>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                            <button type="submit" class="btn btn-warning">Guardar cambios</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
export default {
    name: 'empleados',
    data() {
        return {
            types: ['Automovil', 'Motocicleta', 'Vagoneta', 'Bus', 'Camion'],
            areas: ['Produccion', 'Finanzas', 'Contabilidad'],
            new_auto: {
                empleado: '',
                area: '',
                type: '',
                placa: '',
                color: ''
            },
            vehiculosArray: [],

            itemSelected: null,
            indexSelected: null,
            typeFilter: '',
            toFilter: '',
            toSearch: ''
        }
    },

    mounted() {
        this.fetchVehiculos();
    },
    methods: {
        async fetchVehiculos() {
            try {
                const response = await fetch('http://localhost:3000/vehiculos');
                this.vehiculosArray = await response.json();
            } catch (error) {
                console.error("Error al obtener datos:", error);
            }
        },
        async save() {
            if (this.new_auto.empleado && this.new_auto.area && this.new_auto.type && this.new_auto.placa && this.new_auto.color) {
                try {
                    const response = await fetch('http://localhost:3000/vehiculos', {
                        method: "POST",
                        headers: { "Content-Type": "application/json" },
                        body: JSON.stringify(this.new_auto)
                    });
                    const nuevoVehiculo = await response.json();
                    this.vehiculosArray.push(nuevoVehiculo);

                    this.new_auto = { empleado: '', area: '', type: '', placa: '', color: '' };
                } catch (error) {
                    console.error("Error al agregar vehículo:", error);
                }
            } else {
                alert("Todos los campos son obligatorios.");
            }
        },
        async remove(index) {
            const vehiculo = this.vehiculosArray[index];
            if (confirm("¿Está seguro de eliminar este ítem?")) {
                try {
                    await fetch(`http://localhost:3000/vehiculos/${vehiculo.id}`, { method: "DELETE" });
                    this.vehiculosArray.splice(index, 1);
                } catch (error) {
                    console.error("Error al eliminar vehículo:", error);
                }
            }
        },
        edit(index) {
            this.itemSelected = { ...this.vehiculosArray[index] };
            this.indexSelected = index;
            const editModal = new bootstrap.Modal(document.getElementById('editModal'));
            editModal.show();
        },
        async saveEdit() {
            if (!this.itemSelected) return;
            try {
                await fetch(`http://localhost:3000/vehiculos/${this.itemSelected.id}`, {
                    method: "PUT",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify(this.itemSelected)
                });

                this.vehiculosArray[this.indexSelected] = { ...this.itemSelected };

                const modalElement = document.getElementById('editModal');
                const modalInstance = bootstrap.Modal.getInstance(modalElement) || new bootstrap.Modal(modalElement);
                modalInstance.hide();

                this.itemSelected = null;
                this.indexSelected = null;
                alert('Datos Guardados');
            } catch (error) {
                console.error("Error al actualizar vehículo:", error);
            }
        },
        getList() {
            return this.vehiculosArray.filter((item) => {
                return (!this.toSearch || item.empleado.includes(this.toSearch)) &&
                    (!this.toFilter || item.type === this.toFilter);
            });
        }
    }
}
</script>

<style>
.btn {
    margin-right: 3px;
}

.card {
    overflow-x: auto;
}
</style>