<template>
<div class="LeftSideBar">
    <v-main style="--v-layout-top: 0px;">
        <v-navigation-drawer rail color="rgb(2, 42, 56)">
            <v-list>
                <!-- Layers Button -->
                <v-list-item @click="openLeftDrawer('layers')" class="cursor-pointer">
                    <v-tooltip location="left">
                        <template v-slot:activator="{ props: tooltip }">
                            <v-icon style="color: wheat;" v-bind="tooltip">mdi-layers-triple</v-icon>
                        </template>
                        <span>Layers</span>
                    </v-tooltip>
                </v-list-item>
                <v-divider :thickness="2" color="white"></v-divider>
                <!-- Time Series Button -->
                <v-list-item @click="openLeftDrawer('timeSeries')" class="cursor-pointer">
                    <v-tooltip location="left">
                        <template v-slot:activator="{ props: tooltip }">
                            <v-icon style="color: wheat;" v-bind="tooltip">mdi-chart-timeline</v-icon>
                        </template>
                        <span>Time Series</span>
                    </v-tooltip>
                </v-list-item>
                <v-divider :thickness="2" color="white"></v-divider>
                <!-- Feature Info Button -->
                <v-list-item @click="openLeftDrawer('featureInfo')" class="cursor-pointer">
                    <v-list-item-title>
                        <v-tooltip location="right">
                            <template v-slot:activator="{ props: tooltip }">
                                <v-icon style="color: wheat;" v-bind="tooltip">mdi-information</v-icon>
                            </template>
                            <span>Feature Info</span>
                        </v-tooltip>
                    </v-list-item-title>
                </v-list-item>
                <v-divider :thickness="2" color="white"></v-divider>
                <!-- Compare Button -->
                <v-list-item @click="openLeftDrawer('compare')" class="cursor-pointer">
                    <v-list-item-title>
                        <v-tooltip location="right">
                            <template v-slot:activator="{ props: tooltip }">
                                <v-icon style="color: wheat;" v-bind="tooltip">mdi-compare</v-icon>
                            </template>
                            <span>Compare Tool</span>
                        </v-tooltip>
                    </v-list-item-title>
                </v-list-item>
                <v-divider :thickness="2" color="white"></v-divider>
                <!-- Elevation Profile Button -->
                <v-list-item @click="openLeftDrawer('elevationProfile')" class="cursor-pointer">
                    <v-list-item-title>
                        <v-tooltip location="right">
                            <template v-slot:activator="{ props: tooltip }">
                                <v-icon style="color: wheat;" v-bind="tooltip">mdi-chart-areaspline</v-icon>
                            </template>
                            <span>Elevation Profile</span>
                        </v-tooltip>
                    </v-list-item-title>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>

        <!-- Dynamic Drawer -->
        <v-navigation-drawer v-model="isLeftDrawerOpen" location="left" :width="250">
            <!-- Layers Content -->
            <div v-if="activeLeftTab === 'layers'">
                <v-list>
                    <v-list-item>
                    <div class="text-h5" style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center;">Layers</div>
                </v-list-item>
                <v-divider :thickness="2" color="black"></v-divider>
                    <v-expansion-panels>
                        <v-expansion-panel elevation="0">
                            <v-expansion-panel-title style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center; font-size:15px">
                                Administrative Boundaries
                            </v-expansion-panel-title>
                            <v-expansion-panel-text>
                                <!-- Boundary Layer Checkboxes -->
                                <v-list-item v-for="(adminLayer, index) in adminLayers" :key="index">
                                    <v-checkbox color="primary" :label="adminLayer.name" v-model="adminLayer.visible" 
                                    style="margin-top: -7px; margin-bottom: -38px; margin-left:-10px" 
                                    @change="onAdminBoundaryVisibilityChange(adminLayer)"></v-checkbox>
                                </v-list-item>
                            </v-expansion-panel-text>
                        </v-expansion-panel>
                        <v-expansion-panel elevation="0">
                            <v-expansion-panel-title style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center; font-size:15px">
                                Project Boundaries
                            </v-expansion-panel-title>
                            <v-expansion-panel-text>
                                <!-- Project Layer Checkboxes -->
                                <v-list-item v-for="(projectLayer, index) in projectLayers" :key="index">
                                    <v-checkbox color="primary" :label="projectLayer.name" v-model="projectLayer.visible" 
                                    style="margin-top: -7px; margin-bottom: -38px; margin-left:-10px" 
                                    @change="onProjectBoundaryVisibilityChange(projectLayer)"></v-checkbox>
                                </v-list-item>
                            </v-expansion-panel-text>
                        </v-expansion-panel>
                    </v-expansion-panels>
                    <v-divider></v-divider>
                </v-list>
            </div>
            <!-- Time Series Content -->
            <div v-else-if="activeLeftTab === 'timeSeries'">
                <v-list-item>
                    <div class="text-h5" style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center;">Time Series Data</div>
                </v-list-item>
                <v-divider :thickness="2" color="black"></v-divider>
                <v-select style="margin-top: 40px; margin-bottom: 30px;" v-model="timeSeries" :items="timeSeriesLayers" label="Please Select an Option" required></v-select>
                <v-divider :thickness="3"></v-divider>
            </div>
            <!-- Feature Info Content -->
            <div v-else-if="activeLeftTab === 'featureInfo'">
                <v-card elevation="0">
                    <v-card-title>
                        <div class="text-h5" style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center;">Feature Info Tool</div>
                    </v-card-title>
                    <v-divider :thickness="2" color="black"></v-divider>
                    <v-card-text>
                        <v-switch color="error" v-model="featureInfoEnabled" label="Enable Feature Info" @change="toggleFeatureInfo"></v-switch>
                    </v-card-text>
                </v-card>
                <v-divider :thickness="3"></v-divider>  
            </div>
            <!-- Compare Tool Content -->
            <div v-else-if="activeLeftTab === 'compare'">
                <v-card elevation="0">
                    <v-card-title>
                        <div class="text-h5" style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center;">Comparision Tool</div>
                    </v-card-title>
                    <v-divider :thickness="2" color="black"></v-divider>
                <v-card-text>
                    <v-title>
                        <div class="text-h8 font-weight-bold">Left Side</div>
                    </v-title>
                    <v-select v-model="leftSelect" :items="months" label="Please Select a Layer" @change="validateSelection"></v-select>
                    <v-title>
                        <div class="text-h8 font-weight-bold">Right Side</div>
                    </v-title>
                    <v-select v-model="rightSelect" :items="months" label="Please Select a Layer" @change="validateSelection"></v-select>
                    <v-btn  :disabled="!isBothSelected || showError" :color="comparisonStarted ? 'red' : 'green'" @click="toggleComparison">{{ comparisonStarted ? 'Disable Compare' : 'Compare' }} </v-btn>
                    <div v-if="showError" style="font-size: 15px; color: red; margin-top: 20px;">Please Select Different Layers.</div>
                </v-card-text>
            </v-card>
                <v-divider :thickness="3"></v-divider> 
            </div>
            <div v-else-if="activeLeftTab === 'elevationProfile'">
                <v-card elevation="0">
                    <v-card-title>
                        <div class="text-h5" style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center;">Elevation Profile</div>
                    </v-card-title>
                    <v-divider :thickness="2" color="black"></v-divider>
                    <v-card-text>
                        <v-select v-model="elevationProfile" :items="elevationProfileLayers" label="Please Select an Option" required></v-select>
                    </v-card-text>
                </v-card>
                <v-divider :thickness="3"></v-divider>
            </div>
        </v-navigation-drawer>
    </v-main>
</div>
</template>

<script>
import eventBus from '@/event-bus';

export default {
    name: 'LeftSideBar',

    data() {
        return {
            // Left 
            isLeftDrawerOpen: false,
            activeLeftTab: null,
            featureInfoEnabled: false,
            adminLayers: [
                            { name: 'India Boundary', visible: true },
                            { name: 'States', visible: false },
                            { name: 'Districts', visible: false },
                            { name: 'Sub-Districts', visible: false },
                        ],
            projectLayers: [
                            { name: 'Ganga Basin', visible: true },
                        ],
            leftSelect: null,
            rightSelect: null,
            months: ["Evapotranspiration", "Precipitation"],
            comparisonStarted: false, 
        };
    },
    computed: {
        isBothSelected() {
            return this.leftSelect && this.rightSelect;
        },
        showError() {
            return this.leftSelect === this.rightSelect;
        },
    },

    methods: {
        openLeftDrawer(leftTab) {
            if (this.activeLeftTab === leftTab && this.isLeftDrawerOpen) {
                this.isLeftDrawerOpen = false;
                this.activeLeftTab = null;
            } else {
                this.activeLeftTab = leftTab;
                this.isLeftDrawerOpen = true;
            }
        },
        onAdminBoundaryVisibilityChange(adminLayer) {
                eventBus.emit('toggle-layer-visibility', {name: adminLayer.name,visible: adminLayer.visible,});
        },
        onProjectBoundaryVisibilityChange(projectLayer) {
                eventBus.emit('toggle-layer-visibility', {name: projectLayer.name,visible: projectLayer.visible,});
        },
        toggleFeatureInfo() {
            eventBus.emit('toggleFeatureInfo', this.featureInfoEnabled);
            if (this.featureInfoEnabled) {
                document.body.style.cursor = 'grab';  
            } else {
                document.body.style.cursor = 'auto'; 
            }
        if (!this.featureInfoEnabled) {
        eventBus.emit('hidePopup');
            }
        },
        validateSelection() {
                this.showError = this.leftSelect === this.rightSelect;
            },
        toggleComparison() {
                if (!this.comparisonStarted) {
                    if (!this.showError) {
                        this.comparisonStarted = true;
                        eventBus.emit("compare-layers", {left: this.leftSelect,right: this.rightSelect,});
                    }
                } else {
                    this.comparisonStarted = false;
                    eventBus.emit("remove-comparison");
                }
        },
    },
};
</script>
