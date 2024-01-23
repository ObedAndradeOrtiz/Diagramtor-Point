<template>
    <Head title="About us" />

    <AuthenticatedLayout>
        <template #header>
            Diagrama: {{ project.name }}
        </template>
        <!-- <nav class="border-b text-sm flex justify-start -mt-4 mb-3">
            <button class="inline-block px-4 py-2"
                :class="[currentTab === 1 ? 'border-b-2 border-purple-700 text-purple-700 font-semibold' : 'text-gray-700 hover:text-black']"
                @click="changeTab(1)">
                Diagramas
            </button>
            <button class="inline-block px-4 py-2 border-b-2"
                :class="[currentTab === 2 ? 'border-b-2 border-purple-700 text-purple-700 font-semibold' : 'text-gray-700 hover:text-black']"
                @click="changeTab(2)">
                Colaboradores
            </button>
        </nav> -->
        <div>
            <div style="display: flex; justify-content: end;" v-if="props.project.user_id == $page.props.auth.user.id">
                <PrimaryButton @click="showCreateForm">
                    <div class="flex">
                        Crear Diagrama
                    </div>
                </PrimaryButton>
            </div>
            <table class="w-full text-sm text-left text-gray-500 mt-5 shadow-sm border">
                <thead class="text-xs text-gray-700 uppercase bg-gray-50">
                    <tr>
                        <th scope="col" class="px-6 py-3">
                            Nombre
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Descripción
                        </th>
                        <th scope="col" class="px-6 py-3">
                            <span class="sr-only">Edit</span>
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr class="bg-white border-b" v-for="item in diagrams" :key="item.id" v-if="diagrams.length">
                        <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap">
                            <a :href="route('diagrams.show', item.id)" class="hover:text-violet-700"> {{ item.name
                            }}</a>
                        </th>
                        <td class="px-6 py-4">
                            {{ item.description }}
                        </td>
                        <td class="px-6 py-4 text-right">
                            <button class="font-medium text-blue-600 dark:text-blue-500 hover:underline"
                                @click="showEditForm(item)">
                                <svg class="icon-20" width="20" viewBox="0 0 24 24" fill="none"
                                    xmlns="http://www.w3.org/2000/svg">
                                    <path
                                        d="M11.4925 2.78906H7.75349C4.67849 2.78906 2.75049 4.96606 2.75049 8.04806V16.3621C2.75049 19.4441 4.66949 21.6211 7.75349 21.6211H16.5775C19.6625 21.6211 21.5815 19.4441 21.5815 16.3621V12.3341"
                                        stroke="currentColor" stroke-width="1.5" stroke-linecap="round"
                                        stroke-linejoin="round"></path>
                                    <path fill-rule="evenodd" clip-rule="evenodd"
                                        d="M8.82812 10.921L16.3011 3.44799C17.2321 2.51799 18.7411 2.51799 19.6721 3.44799L20.8891 4.66499C21.8201 5.59599 21.8201 7.10599 20.8891 8.03599L13.3801 15.545C12.9731 15.952 12.4211 16.181 11.8451 16.181H8.09912L8.19312 12.401C8.20712 11.845 8.43412 11.315 8.82812 10.921Z"
                                        stroke="currentColor" stroke-width="1.5" stroke-linecap="round"
                                        stroke-linejoin="round"></path>
                                    <path d="M15.1655 4.60254L19.7315 9.16854" stroke="currentColor" stroke-width="1.5"
                                        stroke-linecap="round" stroke-linejoin="round"></path>
                                </svg>
                            </button>
                            <button class="font-medium text-red-600 hover:underline mx-2"
                                @click="deleteDiagram(item.id, item.name)">
                                <svg class="icon-20" width="20" viewBox="0 0 24 24" fill="none"
                                    xmlns="http://www.w3.org/2000/svg">
                                    <path opacity="0.4"
                                        d="M16.34 1.99976H7.67C4.28 1.99976 2 4.37976 2 7.91976V16.0898C2 19.6198 4.28 21.9998 7.67 21.9998H16.34C19.73 21.9998 22 19.6198 22 16.0898V7.91976C22 4.37976 19.73 1.99976 16.34 1.99976Z"
                                        fill="currentColor"></path>
                                    <path
                                        d="M15.0158 13.7703L13.2368 11.9923L15.0148 10.2143C15.3568 9.87326 15.3568 9.31826 15.0148 8.97726C14.6728 8.63326 14.1198 8.63426 13.7778 8.97626L11.9988 10.7543L10.2198 8.97426C9.87782 8.63226 9.32382 8.63426 8.98182 8.97426C8.64082 9.31626 8.64082 9.87126 8.98182 10.2123L10.7618 11.9923L8.98582 13.7673C8.64382 14.1093 8.64382 14.6643 8.98582 15.0043C9.15682 15.1763 9.37982 15.2613 9.60382 15.2613C9.82882 15.2613 10.0518 15.1763 10.2228 15.0053L11.9988 13.2293L13.7788 15.0083C13.9498 15.1793 14.1728 15.2643 14.3968 15.2643C14.6208 15.2643 14.8448 15.1783 15.0158 15.0083C15.3578 14.6663 15.3578 14.1123 15.0158 13.7703Z"
                                        fill="currentColor"></path>
                                </svg>
                            </button>
                        </td>
                    </tr>
                    <tr class="bg-white border-b" v-else>
                        <th scope="row" colspan="3"
                            class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap text-center">
                            No hay diagramas
                        </th>
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="py-5">
            <div style="display: flex; justify-content: end;" v-if="props.project.user_id == $page.props.auth.user.id">
                <PrimaryButton @click="showAddColabForm">
                    <div class="flex">

                        Añadir colaborador
                    </div>
                </PrimaryButton>
            </div>
            <table class="w-full text-sm text-left text-gray-500 mt-5 shadow-sm border">
                <thead class="text-xs uppercase bg-gray-50">
                    <tr>
                        <th scope="col" class="px-6 py-3">
                            Nombre
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Descripción
                        </th>
                        <th scope="col" class="px-6 py-3">
                            Acciones
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr class="bg-white border-b text-gray-900" v-for="user in collaborators" :key="user.id"
                        v-if="collaborators.length">
                        <td class="px-6 py-4">
                            {{ user.name }} <span class="text-gray-600">{{ + user.id == props.project.user_id ? '(Creador)' :
                                '' }}</span>
                        </td>
                        <td class="px-6 py-4">
                            {{ user.email }}
                        </td>
                        <td class="px-6 py-4">
                            <button class="font-medium text-red-600 hover:underline mx-2"
                                v-if="props.project.user_id == $page.props.auth.user.id && user.id != props.project.user_id" @click="dismissUser(user.id, user.name)">
                                <svg class="icon-20" width="20" viewBox="0 0 24 24" fill="none"
                                    xmlns="http://www.w3.org/2000/svg">
                                    <path opacity="0.4"
                                        d="M16.34 1.99976H7.67C4.28 1.99976 2 4.37976 2 7.91976V16.0898C2 19.6198 4.28 21.9998 7.67 21.9998H16.34C19.73 21.9998 22 19.6198 22 16.0898V7.91976C22 4.37976 19.73 1.99976 16.34 1.99976Z"
                                        fill="currentColor"></path>
                                    <path
                                        d="M15.0158 13.7703L13.2368 11.9923L15.0148 10.2143C15.3568 9.87326 15.3568 9.31826 15.0148 8.97726C14.6728 8.63326 14.1198 8.63426 13.7778 8.97626L11.9988 10.7543L10.2198 8.97426C9.87782 8.63226 9.32382 8.63426 8.98182 8.97426C8.64082 9.31626 8.64082 9.87126 8.98182 10.2123L10.7618 11.9923L8.98582 13.7673C8.64382 14.1093 8.64382 14.6643 8.98582 15.0043C9.15682 15.1763 9.37982 15.2613 9.60382 15.2613C9.82882 15.2613 10.0518 15.1763 10.2228 15.0053L11.9988 13.2293L13.7788 15.0083C13.9498 15.1793 14.1728 15.2643 14.3968 15.2643C14.6208 15.2643 14.8448 15.1783 15.0158 15.0083C15.3578 14.6663 15.3578 14.1123 15.0158 13.7703Z"
                                        fill="currentColor"></path>
                                </svg>
                            </button>
                        </td>
                    </tr>
                    <tr class="bg-white border-b" v-else>
                        <th scope="row" colspan="3"
                            class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap text-center">
                            No hay colaboradores
                        </th>
                    </tr>
                </tbody>
            </table>
        </div>
    </AuthenticatedLayout>
    <div v-if="isCreateFormShow" class="form-cover">
        <DiagramForm :close="closeForm" :project="props.project" :op="1" />
    </div>
    <div v-if="isEditFormShow" class="form-cover">
        <DiagramForm :close="closeForm" :project="props.project" :op="2" :diagram="diagramSelected" />
    </div>
    <div v-if="isInvModalShow" class="form-cover">
        <InvitationModal :close="closeForm" :project_id="props.project.id" />
    </div>
</template>

<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head, useForm } from '@inertiajs/vue3';
import { ref, onMounted } from 'vue';
import Swal from 'sweetalert2';
import DiagramForm from '@/Components/DiagramForm.vue'
import InvitationModal from '@/Components/InvitationModal.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';

const props = defineProps({
    project: { type: Object },
    diagrams: { type: Object },
})

const form = useForm({});
const collaborators = ref([]);
let isCreateFormShow = ref(false)
let isEditFormShow = ref(false)
let isInvModalShow = ref(false)
let diagramSelected = ref(null)
let currentTab = ref(1)

function showCreateForm() {
    isCreateFormShow.value = true
}

function showEditForm(diagram) {
    diagramSelected.value = diagram;
    isEditFormShow.value = true
}

function closeForm() {
    isCreateFormShow.value = false;
    isEditFormShow.value = false;
    isInvModalShow.value = false;
    diagramSelected.value = null;
}

function changeTab(tab) {
    currentTab.value = tab
}

function showAddColabForm() {
    isInvModalShow.value = true;
}

const getCollaborators = async () => {
    await axios.get(route('api.projects.collaborators', props.project.id))
        .then((response) => {
            collaborators.value = response.data;
        })
        .catch(error => console.log(error))
}

const deleteDiagram = (id, name) => {
    Swal.fire({
        title: '¿Estás seguro de eliminar el diagrama ' + name + '?',
        text: 'Esta accion es irreversible',
        icon: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Si, eliminar',
        cancelButtonText: 'Cancelar'
    }).then((result) => {
        if (result.isConfirmed) {
            form.delete(route('diagrams.destroy', id));
        }
    })
};

const dismissUser = (id, name) => {
    Swal.fire({
        title: '¿Estás seguro de desvincular a ' + name + '?',
        text: 'Ya no estará en el Diagrama',
        icon: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Si, desvincular',
        cancelButtonText: 'Cancelar'
    }).then((result) => {
        if (result.isConfirmed) {
            axios.delete(route('api.projects.dismiss', { project: props.project.id, user: id })).then((response) => {
                console.log(response.data)
                getCollaborators();
            }).catch(error => {
                console.error(error);
            });
        }
    })
};

onMounted(() => {
    getCollaborators();
});

</script>

<style>
.form-cover {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 999;
    background: rgba(0, 0, 0, 0.5);
}
</style>
