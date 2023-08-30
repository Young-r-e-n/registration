<template>
  <Vue3MultiStepper
      class="max-w-lg border px-11 py-6"
      v-model:step="step"
      :tabs="tabs"
      primaryColor1="#3490dc"
      primaryColor2="#3490dc"
      backText="Previous"
      nextText="Next"
      doneText="Finish"
      :loading="loading"
      :finalize="handleFormSubmission"
      :validateStep="validateStep"
  >
    <template #1>
      <div class="max-w-md mx-auto ">
        <h3 class="font-heading text-center text-2xl text-gray-900 font-semibold mb-4">HELPDESK</h3>

        <form class="space-y-3.5">
          <label class="block text-md text-gray-700 font-semibold text-left" for="name">Name</label>
          <input
              class="w-full py-2 px-4 border border-gray-300 rounded-md focus:ring-blue-500 focus:border-blue-500"
              type="text"
              id="name"
              placeholder="Stefano Bojarski"
          >

          <label class="block text-md text-gray-700 font-semibold text-left" for="email">Email</label>
          <input
              class="w-full py-2 px-4 border border-gray-300 rounded-md focus:ring-blue-500 focus:border-blue-500"
              type="email"
              id="email"
              placeholder="pat@saturn.dev"
          >

          <label class="block text-md text-gray-700 font-semibold text-left" for="password">Password</label>
          <div class="relative">
            <input
                class="w-full py-2 px-4 border border-gray-300 rounded-md focus:ring-blue-500 focus:border-blue-500"
                type="password"
                id="password"
                placeholder="min 12 cars"
            >

          </div>
          <label class="block text-md text-gray-700 font-semibold text-left" for="password">Confirm Password</label>
          <div class="relative">
            <input
                class="w-full py-2 px-4 border border-gray-300 rounded-md focus:ring-blue-500 focus:border-blue-500"
                type="password"
                id="confirmpassword"
                placeholder="min 12 cars"
            >

          </div>


        </form>
      </div>
    </template>

    <template #2>
      <div class="max-w-md mx-auto ">

        <form class="space-y-3.5">
          <label class="block text-md text-gray-700 font-semibold text-left" for="name">Directorate</label>
          <select
              class="w-full py-2 px-4 border border-gray-300 rounded-md focus:ring-blue-500 focus:border-blue-500"
              id="name"
              v-model="selectedDirectorate"
              @change="filterDepartmentOptions"
          >
            <option value="Executive">Executive</option>
            <option value="CS">Corporation Secretary</option>
            <option value="FPS">Forest Protection and
              Security</option>
            <option value="FCM">Forest Conservation
              and Management</option>
            <option value="SPRM">Strategy,Partnership &
              Resource Mobilization</option>
            <option value="COS">Corporate Services </option>
            <option value="IIA">Inspectorate and
              Internal Affairs</option>
            <option value="AW">Air wing</option>
            <option value="IA">Internal Audit</option>

            <!-- Add more options as needed -->
          </select>

          <label class="block text-md text-gray-700 font-semibold text-left" for="email">Department</label>
          <select
              class="w-full py-2 px-4 border border-gray-300 rounded-md focus:ring-blue-500 focus:border-blue-500"
              id="email"
              v-model="selectedDepartment"
              @change="filterSectionOptions"
          >
            <option v-for="option in filteredDepartments" :value="option" :key="option">{{ option }}</option>
          </select>

          <label class="block text-md text-gray-700 font-semibold text-left" for="password">Section</label>
          <select
              class="w-full py-2 px-4 border border-gray-300 rounded-md focus:ring-blue-500 focus:border-blue-500"
              id="password"
              v-model="selectedSection"
          >
            <option v-for="option in filteredSections" :value="option" :key="option">{{ option }}</option>
          </select>
        </form>
      </div>

    </template>

    <template #3>
      <label class="block text-md text-gray-700 font-semibold text-left" for="region">Region</label>
      <select
          class="w-full py-2 px-4 border border-gray-300 rounded-md focus:ring-blue-500 focus:border-blue-500"
          id="region"
          v-model="selectedRegion"
          @change="filterStationOptions"
      >
        <option v-for="region in regions" :value="region" :key="region">{{ region }}</option>
      </select>

      <label class="block text-md text-gray-700 font-semibold text-left" for="station">Station</label>
      <select
          class="w-full py-2 px-4 border border-gray-300 rounded-md focus:ring-blue-500 focus:border-blue-500"
          id="station"
          v-model="selectedStation"
      >
        <option v-for="station in filteredStations" :value="station" :key="station">{{ station }}</option>
      </select>
    </template>


  </Vue3MultiStepper>
</template>


<script>
import { computed } from "vue";
import { Vue3MultiStepper } from "vue3-multi-stepper";

export default {
  components: {
    Vue3MultiStepper,
  },
  name: "App",
  data() {
    return {
      selectedRegion: '',
      selectedStation: '',
      step: 1,
      loading: false,
      // NB: Use computed properties just in case you intend using translations (e.g this.$t)
      tabs: computed(() => {
        return [
          {
            title: "Your Details",
            iconSuccess: null,
            isValid: true,
          },
          {
            title: "Department Details",
            iconSuccess: null,
            isValid: true,
          },
          {
            title: "Security Info",
            iconSuccess: null,
            isValid: true,
          },


        ];

      }),
      regions: ['North Eastern', 'Coast', 'Ewaso North', 'Eastern', 'Central Highlands', 'Mau', 'North Rift', 'Nyanza', 'Western', 'Nairobi', 'KFC'], // Add regions
      regionStations: {
        'North Eastern': ['RCF-North Eastern', 'Garissa', 'Wajir', 'Mandera'],
        'Coast': ['RFC-Coast', 'Mombasa', 'Kwale', 'Tana River', 'Taita Taveta'],
        'Ewaso North': ['RFC-Ewaso North', 'Samburu', 'Isiolo', 'Marsabit'],
        'Eastern': ['RFC-Eastern', 'Machakos', 'Makueni', 'Kitui', 'Embu', 'Meru', 'Tharaka'],
        'Central Highlands': ['RFC-Central Highlands', 'Muranga', 'Kirinyaga', 'Nyeri', 'Laikipia', 'Nyandarua', 'Kiambu'],
        'Mau': ['RFC-Mau', 'Bomet', 'Narok', 'Kericho', 'Nakuru', 'Baringo'],
        'North Rift': ['RFC-North Rift', 'Uasin Gishu', 'Elgeyo', 'Transnzoia', 'Nandi', 'West Pokot', 'Turkana'],
        'Nyanza': ['RFC-Nyanza', 'Kisumu', 'Siaya', 'Migori', 'Homabay', 'Kisii', 'Nyamira'],
        'Western': ['RFC-Western', 'Busia', 'Bungoma', 'Kakamega', 'Vihiga'],
        'Nairobi': ['HOC Nairobi', 'Kajiado', 'Nairobi'],
        'KFC': ['KFC'],},
      selectedDirectorate: '',
      selectedDepartment: '',
      selectedSection: '',
      departments: {
        Executive: ['CCF','CCF Secretariat','CCF PA', 'Board Chair Office','Board Secretariat'],
        CS:['Corporation Secretary','Manager Legal Services','Legal Services'],
        FPS:['SDCCF-FP&S','Protection, Fire and Disaster Operations','Investigations, Prosecution and Inspection','FOLEA','Security and Intelligence'],
        FCM:['Natural Forest Conservation','Survey and Information management','Plantation Management','Conservation coordination','Kenya Forestry College','Forestry Advisory & County Liaison','Drylands & Private forest development'],
        SPRM:['Partnership & Resource Mobilization',
        'Enterprise and Marketing',
        'Planning, M&E and Performance',
        'Management',
        'Quality Assurance and Risk Management'],
        COS:['Finance and Accounting','Human Resource Management','Administration and Infrastructure Devt','Information Communication Technology','Manager Corporate Communication','Corporate Communications','Manager Supply Chain Management','Supply Chain Management'],
        IIA:[''],
        AW:[''],
        IA:['Senior Audit',
        'Internal Audit']


        // Add more departments as needed
      },
      sections: {
        "Natural Forest Conservation":["SDCCF-FCM", "DCCF-FCM",
          "Natural Forest Conservation",
          "Climate Change and conventions",
          "Participatory forest management",
          "Biodiversity & Invasive plants management",
          "Forest Fire management"
        ],
        'Survey and Information management':['Forest Survey and Mapping', 'Forest information management'      ],
        'Plantation Management':['DCCF-Plantation Management',
        'Plantation management',
        'Plantation Establishment',
        'Plantation inventory',
        'Plantation Harvesting programs',
        'Silviculture operations',
        'Forest Health'],
        'Conservation coordination':['Regional forest conservation area',
        'FCC Linkage Office'],
        'Kenya Forestry College':[''],
        'Forestry Advisory & County Liaison':['Manager Forestry Advisory',
        'Forest advisory & capacity building',
        'Research education & county liaison',
        'Forest investment facility'],
        'Drylands & Private forest development':['DCCF-Drylands and Private forests'],
        'Finance and Accounting':['Manager Finance and Accounting',
        'Accountable Documents',
        'Budget',
        'Cash Office',
        'Examinations',
        'Fixed Assets',
        'General Ledger',
        'Imprest',
        'Payables',
        'Reconciliation',
        'Revenue'],
        'Human Resource Management':['Planning and resourcing HRIS-Payroll',
        'Compensation management and',
        'Benefits-Pension',
        'Relations and Welfare programs',
        'Training, Mentorship and Coaching',
        'Performance and HR Audit'],
        'Administration and Infrastructure Devt':['Manager-Administration',
        'Record management and registries',
        'Telephone operator',
        'Fleet Management-Transport',
        'Housing and Maintenance-Building','Forest roads'],
        'Information Communication Technology':['Manager-ICT',
        'Database Administrator',
        'Infrastructure administration',
        'System Admin and Business analyst'],









      // Add more sections as needed
      },
    };
  },
  computed: {
    filteredDepartments() {
      return this.departments[this.selectedDirectorate] || [];
    },
    filteredSections() {
      return this.sections[this.selectedDepartment] || [];
    },
    filteredStations() {
      return this.regionStations[this.selectedRegion] || [];
    },
  },

  methods: {
    filterDepartmentOptions() {
      this.selectedDepartment = ''; // Clear the selected department
      this.selectedSection = ''; // Clear the selected section
    },
    filterSectionOptions() {
      this.selectedSection = ''; // Clear the selected section
    },
    filterStationOptions() {
      this.selectedStation = ''; // Clear the selected station
    },
    async handleFormSubmission() {
      this.loading = true;

      // After 2 seconds, reload the page and start the flow again
      setTimeout(() => {
        location.reload();
      }, 2000);
    },

    validateStep(step) {
      // Perform validation based on the current step
      // Return true if the step is valid, otherwise return false
      if (step === 1) {
        return this.step1Check();
      } else if (step === 2) {
        return this.step2Check();
      } else if (step === 3) {
        return this.step3Check();
      } else if (step === 4) {
        return this.step4Check();
      }

      return true; // Default to true if no validation is implemented
    },

   step1Check() {
      return true;
    },
    step2Check() {
      return true;
    },
    step3Check() {
      return true;
    },
    step4Check() {
      return true;
    },
    step5Check() {
      return true;
    },
  },
};
</script>

<style>
.steppy-item-counter {
  height: 40px !important;
  width: 40px !important;
  border: none !important;
}

.steppy-item-counter .number {
  font-size: 1rem !important;
}

.controls {
  display: flex !important;
  flex-direction: column-reverse !important;
  gap: 2rem !important;
}

.controls .btn {
  align-self: center !important;
}

.btn--default-2 {
  margin: auto !important;
  text-align: center !important;
  background: #f1c446 !important;
  border-radius: 4px !important;
  border: none !important;
  height: 40px !important;
  width: 40% !important;
  font-size: 1rem !important;
  color: #000407 !important;
  font-weight: bold !important;
}
</style>