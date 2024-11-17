<script setup>
import { useLayout } from '@/layout/composables/layout';
import { ProductService } from '@/service/ProductService';
import { onMounted, ref, watch } from 'vue';

const { getPrimary, getSurface, isDarkTheme } = useLayout();

const products = ref(null);
const chartData = ref(null);
const chartOptions = ref(null);

const items = ref([
    { label: 'Add New', icon: 'pi pi-fw pi-plus' },
    { label: 'Remove', icon: 'pi pi-fw pi-trash' }
]);

onMounted(() => {
    ProductService.getProductsSmall().then((data) => (products.value = data));
    chartData.value = setChartData();
    chartOptions.value = setChartOptions();
});

function setChartData() {
    const documentStyle = getComputedStyle(document.documentElement);

    return {
        labels: ['Q1', 'Q2', 'Q3', 'Q4'],
        datasets: [
            {
                type: 'bar',
                label: 'Subscriptions',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-400'),
                data: [4000, 10000, 15000, 4000],
                barThickness: 32
            },
            {
                type: 'bar',
                label: 'Advertising',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-300'),
                data: [2100, 8400, 2400, 7500],
                barThickness: 32
            },
            {
                type: 'bar',
                label: 'Affiliate',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-200'),
                data: [4100, 5200, 3400, 7400],
                borderRadius: {
                    topLeft: 8,
                    topRight: 8
                },
                borderSkipped: true,
                barThickness: 32
            }
        ]
    };
}

function setChartOptions() {
    const documentStyle = getComputedStyle(document.documentElement);
    const borderColor = documentStyle.getPropertyValue('--surface-border');
    const textMutedColor = documentStyle.getPropertyValue('--text-color-secondary');

    return {
        maintainAspectRatio: false,
        aspectRatio: 0.8,
        scales: {
            x: {
                stacked: true,
                ticks: {
                    color: textMutedColor
                },
                grid: {
                    color: 'transparent',
                    borderColor: 'transparent'
                }
            },
            y: {
                stacked: true,
                ticks: {
                    color: textMutedColor
                },
                grid: {
                    color: borderColor,
                    borderColor: 'transparent',
                    drawTicks: false
                }
            }
        }
    };
}



watch([getPrimary, getSurface, isDarkTheme], () => {
    chartData.value = setChartData();
    chartOptions.value = setChartOptions();
});
</script>

<template>
    <div class="card mb-0">
      <div class="flex justify-between mb-4">
        <div>
          <span class="block text-muted-color font-medium mb-4">Upload Orders</span>
          <div class="text-surface-900 dark:text-surface-0 font-medium text-xl">Upload your CSV or Excel file</div>
        </div>
        <div class="flex items-center justify-center bg-blue-100 dark:bg-blue-400/10 rounded-border" style="width: 2.5rem; height: 2.5rem">
          <i class="pi pi-upload text-blue-500 !text-xl"></i>
        </div>
      </div>
      <span class="text-primary font-medium">Select File </span>
      <span class="text-muted-color">and upload your orders file</span>
  
      <!-- File Upload Input -->
      <div class="mt-4">
        <!-- Custom "Choose File" Button as Label -->
        <label for="file-upload" class="choose-file-btn">
          Choose File
        </label>
        <input type="file" id="file-upload" @change="handleFileUpload" class="file-input" />
  
       <!-- Upload Button with Label -->
<button @click="uploadFile" class="mt-2 p-button p-button-primary">
  <span class="mr-2">Upload</span> <!-- Label Text -->
  <i class="pi pi-upload"></i> <!-- Icon (optional) -->
</button>

      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        selectedFile: null,
      };
    },
    methods: {
      handleFileUpload(event) {
        // Handle file selection
        const file = event.target.files[0];
        if (file) {
          this.selectedFile = file;
          console.log("Selected file:", file.name);
        }
      },
      uploadFile() {
        if (this.selectedFile) {
          // Here you would upload the file
          console.log("Uploading file:", this.selectedFile.name);
        } else {
          alert("Please select a file to upload.");
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .file-input {
    display: none; /* Hide the default file input */
  }
  
  /* Custom "Choose File" button styles */
  .choose-file-btn {
    display: inline-block;
    cursor: pointer;
    padding: 10px 20px;
    background-color: #17ed13; /* Customize color */
    color: white;
    border-radius: 4px;
    text-align: center;
    border: 1px solid #007ad9; /* Add border to make it look like a button */
    transition: all 0.3s ease; /* Add a smooth transition */
  }
  
  .choose-file-btn:hover {
    background-color: #005f99; /* Hover state */
    border-color: #1bda06;
  }
  
  .choose-file-btn:active {
    background-color: #21dc0c; /* Active state */
    border-color: #004f7f;
  }
  
  button {
    padding: 10px 20px;
  }
  </style>
  