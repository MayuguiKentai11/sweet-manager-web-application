<script>
import SearchBar from "../../shared/components/search-bar.component.vue";
import TopBarComponent from "../../shared/components/top-bar.component.vue";
import MainPageNavigation from "../components/main-page-navigation.component.vue";

import OverviewIcon from "../../assets/organizational-management/overview-icon.svg";
import AnalyticsIcon from "../../assets/organizational-management/analytics-icon.svg";
import ProvidersIcon from "../../assets/organizational-management/providers-icon.svg";
import InventoryIcon from "../../assets/organizational-management/inventory-icon.svg";
import RoomsIcon from "../../assets/organizational-management/rooms-icon.svg";
import OrganizationIcon from "../../assets/organizational-management/organization-icon.svg";
import DevicesIcon from "../../assets/organizational-management/devices-icon.svg";

export default {
  name: "HotelOverviewPage",
  components: { SearchBar, TopBarComponent, MainPageNavigation },

  data() {
    return {
      hotelId: null,
      navigationItems: [
        {id: "overview", label: "Overview", path: "/home/hotel/1/overview", icon: OverviewIcon, isActive: true},
        {id: "analytics", label: "Analytics", path: "/home/hotel/1/analytics", icon: AnalyticsIcon, isActive: false},
        {id: "providers", label: "Providers", path: "/home/hotel/1/providers", icon: ProvidersIcon, isActive: false},
        {id: "inventory", label: "Inventory", path: "/home/hotel/1/inventory", icon: InventoryIcon, isActive: false},
        {id: "rooms", label: "Rooms", path: "/home/hotel/1/rooms", icon: RoomsIcon, isActive: false},
        {id: "organization", label: "Organization", path: "/home/hotel/1/organization", icon: OrganizationIcon, isActive: false},
        {id: "devices", label: "Devices", path: "/home/hotel/1/set-up/devices", icon: DevicesIcon, isActive: false}
      ],
      hotel: {
        name: "",
        address: "",
        description: "",
        email: "",
        phone: ""
      },
      editable: {
        description: '',
        email: '',
        phone: ''
      },
      editing: {
        description: false,
        email: false,
        phone: false
      }
    };
  },

  mounted() {
    this.hotelId = 1; // this.$route.params.hotelId
    this.loadHotelMockData(); // ← Simulamos una petición
  },

  methods: {
    loadHotelMockData() {
      // Aquí iría el fetch al backend con el this.hotelId
      // Por ahora lo simulamos con datos de prueba
      this.hotel = {
        name: "Royal Decameron Punta Sal",
        address: "Av. Panamericana N, Punta Sal 24560",
        description: `Este vibrante hotel todo incluido se ubica entre palmeras en la playa Punta Sal, a 6 km del centro del pueblo y a 89 km del Aeropuerto Tumbes.
Las habitaciones sencillas cuentan con terraza o balcón, pantalla plana y acceso a internet (con cargo); algunos tienen área de visitas.
Los servicios gratuitos incluyen deportes acuáticos, entretenimiento nocturno y alquiler de equipo para la playa, además de las comidas y bebidas en 3 restaurantes y 5 bares. Otros servicios: club nocturno animado, jardines, playa extensa con cabañas, piscina al aire libre, hidromasaje y gimnasio.
• 🙌 Wifi pago
• 🍳 Desayuno incluido
• 🚗 Estacionamiento gratuito
• 👙 Piscina al aire libre`,
        email: "bookings@decameron.com",
        phone: "073 55 4877"
      };
    },

    enableEdit(field) {
      this.editable[field] = this.hotel[field];
      this.editing[field] = true;
      this.$nextTick(() => {
        const input = this.$refs[field];
        if (input) input.focus();
      });
    },

    saveEdit(field) {
      this.hotel[field] = this.editable[field];
      this.editing[field] = false;
      // En el futuro: aquí iría el PUT al backend
    }
  }
};
</script>


<template>
  <MainPageNavigation
      :navigationItems="navigationItems"
  />
  <div class="hotel-container">
     <!-- Izquierda: Título, dirección e imágenes -->
     <div class="hotel-left">
       <h2 class="hotel-title">{{hotel.name}}</h2>
       <p class="hotel-address">{{hotel.address}}</p>

       <div class="image-gallery">
         <img src="/img1.png" alt="Hotel Aéreo" />
         <img src="/img2.png" alt="Habitación" />
         <img src="/img3.png" alt="Playa" class="full-width-image" />
       </div>
     </div>

     <!-- Derecha: Descripción, servicios, contacto -->
     <div class="hotel-right">
       <h2 class="section-title">Find out how others see your hotel!</h2>

       <div class="section">
         <div class="section-header">
           <div>Descripción</div>
           <span class="edit-icon" @click="enableEdit('description')">✎ Edit</span>
         </div>

         <div v-if="editing.description">
           <textarea ref="description" v-model="editable.description" class="edit-area"></textarea>
           <button class="save-button" @click="saveEdit('description')">Save changes</button>
         </div>
         <p v-else class="description" style="white-space: pre-line">
           {{ hotel.description }}
         </p>

       </div>

       <hr style="margin-bottom:0.5rem"/>
       <div class="section">
         <div class="section-header">
           <div>Email Address</div>
           <span class="edit-icon" @click="enableEdit('email')">✎ Edit</span>
         </div>

         <div v-if="editing.email">
           <input
               ref="email"
               v-model="editable.email"
               class="edit-input"
           />
           <button class="save-button" @click="saveEdit('email')">Save changes</button>
         </div>
         <p v-else class="contact">{{ hotel.email }}</p>

       </div>

       <hr style="margin-bottom:0.5rem"/>
       <div class="section">
         <div class="section-header">
           <div>Phone</div>
           <span class="edit-icon" @click="enableEdit('phone')">✎ Edit</span>
         </div>

         <div v-if="editing.phone">
           <input
               ref="phone"
               v-model="editable.phone"
               class="edit-input"
           />
           <button class="save-button" @click="saveEdit('phone')">Save changes</button>
         </div>
         <p v-else class="contact">{{ hotel.phone }}</p>


       </div>
     </div>
   </div>
</template>

<style scoped>
.hotel-container {
  display: flex;
  gap: 1.5rem;
  padding: 2rem;
  font-family: Arial, sans-serif;
}

/* Left Side */
.hotel-left {
  flex: 1;
}

.hotel-title {
  font-size: 2rem;
}

.hotel-address {
  color: #555;
  margin-bottom: 1rem;
}
.edit-area {
  width: 100%;
  min-height: 6rem;
  font-size: 0.875rem;
  font-family: inherit;
  border: 1px solid #ccc;
  border-radius: 0.5rem;
  resize: vertical;
}
.save-button {
  background-color: var(--primary-color, #007bff);
  color: white;
  border: none;
  border-radius: 0.5rem;
  padding: 0.4rem 1rem;
  cursor: pointer;
  font-size: 0.875rem;
}

.edit-input {
  width: 100%;
  font-size: 0.875rem;
  font-family: inherit;
  padding:  0.5rem;
  border: 1px solid #ccc;
  border-radius: 0.5rem;
}


.image-gallery {
  display: grid;
  grid-template-columns: 2fr 1fr;
  grid-template-rows: 11.25rem 11.875rem; /* 180px and 190px */
  gap: 0.625rem; /* 10px */
  border-radius: 0.75rem; /* 12px */
}

.image-gallery img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 0.75rem;
}

.image-gallery img:nth-child(1) {
  grid-column: 1 / 2;
  grid-row: 1 / 2;
}

.image-gallery img:nth-child(2) {
  grid-column: 2 / 3;
  grid-row: 1 / 2;
}

.image-gallery img:nth-child(3) {
  grid-column: 1 / 3;
  grid-row: 2 / 3;
}

/* Right Side */
.hotel-right {
  flex: 1;
}

.section-title {
  font-size: 1rem;
  font-weight: bold;
  margin-bottom: 1rem;
  margin-top: 0.4375rem; /* 7px */
}

.section {
  margin-bottom: 1rem;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.edit-icon {
  font-size: 0.875rem; /* 14px */
  color: #333;
  cursor: pointer;
}

.description {
  font-size: 0.875rem;
  color: #333;
  margin-top: 0.3125rem; /* 5px */
  line-height: 1.5;
}

.amenities {
  margin-top: 0.625rem;
  list-style: none;
  padding: 0;
  font-size: 0.875rem;
}

.amenities li {
  margin-bottom: 0.25rem; /* 4px */
}

.contact {
  font-size: 0.875rem;
  color: #444;
}
</style>
