<script lang="ts" setup>
    import { ref, watch, nextTick } from 'vue';
    import { ElTable } from 'element-plus';

    const singleArmData = [
        // Row 0: Main category headers
        { method: 'Method', bottle_gsr: 'bottle', knife_gsr: 'knife', screwdriver_gsr: 'screwdriver', pan_gsr: 'pan', mug_gsr: 'mug', kettle_gsr: 'kettle', average_gsr: 'average' },
        // Row 1: Sub-headers (GSR/CFR labels)
        { bottle_gsr: 'GSR', bottle_cfr: 'CFR', knife_gsr: 'GSR', knife_cfr: 'CFR', screwdriver_gsr: 'GSR', screwdriver_cfr: 'CFR', pan_gsr: 'GSR', pan_cfr: 'CFR', mug_gsr: 'GSR', mug_cfr: 'CFR', kettle_gsr: 'GSR', kettle_cfr: 'CFR', average_gsr: 'GSR', average_cfr: 'CFR' },
        // Row 2: gsamgraspnet data
        { method: 'gsamgraspnet',
          bottle_gsr: '0.767', bottle_cfr: '0.700',
          knife_gsr: '0.667', knife_cfr: '0.533',
          screwdriver_gsr: '0.700', screwdriver_cfr: '0.800',
          pan_gsr: '0.367', pan_cfr: '0.267',
          mug_gsr: '0.233', mug_cfr: '0.200',
          kettle_gsr: '0.300', kettle_cfr: '0.133',
          average_gsr: '0.538', average_cfr: '0.443'
        },
        // Row 3: thinkgrasp data
        { method: 'thinkgrasp',
          bottle_gsr: '0.833', bottle_cfr: '0.767',
          knife_gsr: '0.800', knife_cfr: '0.700',
          screwdriver_gsr: '0.833', screwdriver_cfr: '0.733',
          pan_gsr: '0.600', pan_cfr: '0.767',
          mug_gsr: '0.500', mug_cfr: '0.600',
          kettle_gsr: '0.733', kettle_cfr: '0.533',
          average_gsr: '0.705', average_cfr: '0.667'
        },
        // Row 4: Our data
        { method: 'Our',
          bottle_gsr: '0.900', bottle_cfr: '0.933',
          knife_gsr: '0.900', knife_cfr: '0.967',
          screwdriver_gsr: '0.867', screwdriver_cfr: '1.000',
          pan_gsr: '0.900', pan_cfr: '1.000',
          mug_gsr: '0.833', mug_cfr: '0.800',
          kettle_gsr: '0.833', kettle_cfr: '0.800',
          average_gsr: '0.876', average_cfr: '0.900'
        },
    ];

    const dualArmData = [
        { method: 'Method', basin_gsr: 'basin', keyboard_gsr: 'key_board', laptop_gsr: 'laptop', pot_gsr: 'pot', average_gsr: 'Average' },
        { basin_gsr: 'GSR', basin_cfr: 'CFR', keyboard_gsr: 'GSR', keyboard_cfr: 'CFR', laptop_gsr: 'GSR', laptop_cfr: 'CFR', pot_gsr: 'GSR', pot_cfr: 'CFR', average_gsr: 'GSR', average_cfr: 'CFR' },
        { method: 'GroundedSAM + CGDF', basin_gsr: '19/30', basin_cfr: '9/30', keyboard_gsr: '21/30', keyboard_cfr: '10/30', laptop_gsr: '11/30', laptop_cfr: '9/30', pot_gsr: '6/30', pot_cfr: '4/30', average_gsr: '0.475', average_cfr: '0.267' },
        { method: 'OUR', basin_gsr: '24/30', basin_cfr: '26/30', keyboard_gsr: '23/30', keyboard_cfr: '25/30', laptop_gsr: '23/30', laptop_cfr: '24/30', pot_gsr: '22/30', pot_cfr: '27/30', average_gsr: '0.767', average_cfr: '0.850' },
    ];

    const activeTab = ref('Single-Arm');
    const singleArmTableRef = ref<InstanceType<typeof ElTable>>();
    const dualArmTableRef = ref<InstanceType<typeof ElTable>>();

    const handleSingleArmSpan = ({ row, column, rowIndex, columnIndex }: any) => {
        if (rowIndex === 0) {
            if (columnIndex === 0) {
                return { rowspan: 2, colspan: 1 }; // Merge Method column
            }
            // Merge all other columns in first row
            if (columnIndex % 2 === 1) { // First sub-column
                return { rowspan: 1, colspan: 2 };
            } else { // Second sub-column
                return { rowspan: 0, colspan: 0 };
            }
        } else if (rowIndex === 1) {
            if (columnIndex === 0) {
                return { rowspan: 0, colspan: 0 }; // Hide merged Method cell
            }
        }
    };

    const handleDualArmSpan = ({ row, column, rowIndex, columnIndex }: any) => {
        if (rowIndex === 0) {
            if (columnIndex === 0) {
                return { rowspan: 2, colspan: 1 }; // Merge Method column
            }
            if (columnIndex % 2 === 1) { // First sub-column
                return { rowspan: 1, colspan: 2 };
            } else { // Second sub-column
                return { rowspan: 0, colspan: 0 };
            }
        } else if (rowIndex === 1) {
            if (columnIndex === 0) {
                return { rowspan: 0, colspan: 0 };
            }
        }
    };

    watch(activeTab, async (newTab) => {
        await nextTick();
        singleArmTableRef.value?.doLayout();
        dualArmTableRef.value?.doLayout();
    });
</script>

<template>
    <div>
        <el-divider />
        <el-row justify="center">
            <h1 class="section-title" style="font-size: 1.5em; font-weight: bold; margin-bottom: 15px;">More Results</h1>
        </el-row>
        <el-row justify="center">
            <el-col :xs="24" :sm="24" :md="24" :lg="22" :xl="20">
                <el-card class="card">
                    <el-tabs class="demo-tabs" v-model="activeTab">
                        <el-tab-pane label="Single-Arm" name="Single-Arm">
                            <el-table ref="singleArmTableRef"
                                      :data="singleArmData"
                                      :span-method="handleSingleArmSpan"
                                      style="width: 100%"
                                      border
                                      scrollbar-always-on
                                      :show-header="false"
                                      class="single-arm-table">
                                <el-table-column prop="method" width="180" fixed align="left" />
                                <el-table-column label="Bottle" width="180" align="center">
                                    <el-table-column prop="bottle_gsr" width="90" align="center" />
                                    <el-table-column prop="bottle_cfr" width="90" align="center" />
                                </el-table-column>
                                <el-table-column label="Knife" width="180" align="center">
                                    <el-table-column prop="knife_gsr" width="90" align="center" />
                                    <el-table-column prop="knife_cfr" width="90" align="center" />
                                </el-table-column>
                                <el-table-column label="Screwdriver" width="180" align="center">
                                    <el-table-column prop="screwdriver_gsr" width="90" align="center" />
                                    <el-table-column prop="screwdriver_cfr" width="90" align="center" />
                                </el-table-column>
                                <el-table-column label="Pan" width="180" align="center">
                                    <el-table-column prop="pan_gsr" width="90" align="center" />
                                    <el-table-column prop="pan_cfr" width="90" align="center" />
                                </el-table-column>
                                <el-table-column label="Mug" width="180" align="center">
                                    <el-table-column prop="mug_gsr" width="90" align="center" />
                                    <el-table-column prop="mug_cfr" width="90" align="center" />
                                </el-table-column>
                                <!-- Pliers column removed -->
                                <el-table-column label="Kettle" width="180" align="center">
                                    <el-table-column prop="kettle_gsr" width="90" align="center" />
                                    <el-table-column prop="kettle_cfr" width="90" align="center" />
                                </el-table-column>
                                <el-table-column label="average value" width="180" align="center">
                                    <el-table-column prop="average_gsr" width="90" align="center" />
                                    <el-table-column prop="average_cfr" width="90" align="center" />
                                </el-table-column>
                            </el-table>
                        </el-tab-pane>

                        <el-tab-pane label="Dual-Arm" name="Dual-Arm">
                            <el-table ref="dualArmTableRef"
                                      :data="dualArmData"
                                      :span-method="handleDualArmSpan"
                                      style="width: 100%"
                                      border
                                      scrollbar-always-on
                                      :show-header="false"
                                      class="dual-arm-table">
                                <el-table-column prop="method" width="180" fixed align="left" />
                                <el-table-column label="Basin" width="180" align="center">
                                    <el-table-column prop="basin_gsr" width="90" align="center" />
                                    <el-table-column prop="basin_cfr" width="90" align="center" />
                                </el-table-column>
                                <el-table-column label="keyboard" width="180" align="center">
                                    <el-table-column prop="keyboard_gsr" width="90" align="center" />
                                    <el-table-column prop="keyboard_cfr" width="90" align="center" />
                                </el-table-column>
                                <el-table-column label="laptop" width="180" align="center">
                                    <el-table-column prop="laptop_gsr" width="90" align="center" />
                                    <el-table-column prop="laptop_cfr" width="90" align="center" />
                                </el-table-column>
                                <el-table-column label="pot" width="180" align="center">
                                    <el-table-column prop="pot_gsr" width="90" align="center" />
                                    <el-table-column prop="pot_cfr" width="90" align="center" />
                                </el-table-column>
                                <el-table-column label="Average" width="180" align="center">
                                    <el-table-column prop="average_gsr" width="90" align="center" />
                                    <el-table-column prop="average_cfr" width="90" align="center" />
                                </el-table-column>
                            </el-table>
                        </el-tab-pane>
                    </el-tabs>
                </el-card>
            </el-col>
        </el-row>
    </div>
</template>

<style scoped>
    .card {
        margin-top: 20px;
        margin-bottom: 20px;
    }

    .demo-tabs > .el-tabs__content {
        padding: 15px;
        overflow: visible;
    }

    :deep(.el-table) * {
        box-sizing: border-box;
    }

    :deep(.el-table__body),
    :deep(.el-table__footer),
    :deep(.el-table__header) {
        table-layout: fixed;
        width: 100%;
    }

    :deep(.el-table th.el-table__cell),
    :deep(.el-table td.el-table__cell) {
        padding: 8px 5px;
        white-space: normal;
        word-break: break-word;
        vertical-align: middle;
    }

    :deep(.el-table td.el-table__cell) {
        text-align: center;
    }

    :deep(.el-table__body td.el-table__cell:first-child),
    :deep(.el-table__fixed .el-table__body td.el-table__cell:first-child) {
        text-align: left;
        padding-left: 10px;
    }

    /* Center merged cells in first row */
    :deep(.el-table__row:first-child td.el-table__cell) {
        text-align: center !important;
        font-weight: bold;
    }
</style>