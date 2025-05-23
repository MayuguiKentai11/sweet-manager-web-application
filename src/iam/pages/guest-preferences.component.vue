<script>
import BreadCrumb from '../../shared/components/breadcrumb.component.vue';
import AccountDetailFormEdit from '../components/account-detail-form-edit.component.vue';
import AccountInfoOverview from '../components/account-info-overview.component.vue';
import ModalComponent from '../../shared/components/modal.component.vue';
import ButtonComponent from '../../shared/components/button.component.vue';
import userMock from '../../mocks/iam/user-profile-account.json';
import i18n from "../../i18n.js";

export default {
    name: "GuestPreferencesPage",
    computed: {
      i18n() {
        return i18n
      }
    },
    components: {
        BreadCrumb,
        AccountDetailFormEdit,
        AccountInfoOverview,
        ModalComponent,
        ButtonComponent
    },
    data() {
      return {
        userData: null,
        showModal: false,
        breadcrumbPath: [
          { name: "Account", route: "" },
          { name: "Personal Information", route: "" },
          { name: "My Preferences", route: "" },
        ]
      };
    },
    mounted() {
      this.fetchUserData();
    },
    methods: {
      fetchUserData() {
        setTimeout(() => {
            this.userData = userMock;
            console.log("User data fetched:", this.userData);


            this.breadcrumbPath[0].route = `/home/profile/${this.userData.id}`;
            this.breadcrumbPath[1].route = `/home/profile/${this.userData.id}/account`;
            this.breadcrumbPath[2].route = `/home/profile/${this.userData.id}/preferences`;
        }, 300);
      },
      saveField(field, value) {
        console.log("Saving field:", field, "New value:", value);
        this.userData.preferences[field] = value; 
      },
      editField(field, value) {
        console.log("Editing field:", field, "Current value:", value);
      },
      handleModal(action = 'toggle') {
        if (action === 'open') {
          this.showModal = true;
        } else if (action === 'close') {
          this.showModal = false;
        } else {
          this.showModal = !this.showModal;
        }
      }

    }
}
</script>

<template>
  
  <ModalComponent v-model="showModal" title="Request Room Card" :showCloseButton="true" :closeOnOverlayClick="true" width="350px" height="auto" class="modal">
    <template #image>
      <img src="../../assets/iam/card_id_room.svg" alt="Card ID" class="icon-card-id" />
    </template>
    <template #header>
      <h1>{{ i18n.global.t('guest-preferences.modal-component.header')}}</h1>
    </template>
    <template #body>
      <p>{{ i18n.global.t('guest-preferences.modal-component.body') }}</p>
    </template>
    <template #footer>
      <ButtonComponent :state="'basic'" :text="'Cancel'" :onClick="() => handleModal('close')" :width="'50%'"/>
      <ButtonComponent :state="'primary'" :text="'Request'" :onClick="() => handleModal('close')" :width="'50%'"/>
    </template>
  </ModalComponent>

 <div class="user-preferences-container">
    <BreadCrumb :path="breadcrumbPath" />

  <div class="user-preferences-content">
    <AccountInfoOverview v-if="userData" :username="userData.name" :userType="userData.type" class="user-overview" />

    <div class="form-edit">

      <AccountDetailFormEdit v-if="userData" :userData="userData.preferences" :notShowFields="['id', 'type', 'created_at', 'updated_at', 'image']" @edit="editField" @save="saveField" />

      <div class="account-detail-item">
        <div class="item-fields">
          <p class="account-detail-item__field">{{ i18n.global.t('guest-preferences.item-fields.account-detail-item__field') }} </p>
          <p class="account-detail-item__field-value">
            {{ i18n.global.t('guest-preferences.item-fields.account-detail-item__field-value') }}
          </p>
        </div>

        <div class="item-edit" @click="() => handleModal('open')">
          <img src="/icon_open.svg" alt="Request Icon" class="edit-icon" />
          <p class="account-detail-item__edit">
            {{ i18n.global.t('guest-preferences.item-fields.account-detail-item__edit') }}
          </p>
        </div>
      </div>
    </div>

    
  </div>
 </div>

</template>

<style scoped>
::v-deep(.modal-image) {
  background-color: transparent;
  height: 150px;
}

::v-deep(.modal-header) {
  padding-top: 0;
}

.user-preferences-container {
  margin: 2rem;
}

.user-preferences-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

.account-path {
  margin: 1rem 0;
  font-size: .8rem;
}

.user-overview {
  width: 100%;
  max-width: 550px;
  margin-top: 2rem;
}

.form-edit {
  width: 100%;
  max-width: 550px;
}


.account-detail-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 0;
  border-bottom: 1px solid var(--gray-extra-light-color);
  width: 100%;
  
}

.item-fields{
  display: flex;
  flex-direction: column;
  padding: 1rem 0;
}

.account-detail-item__field {
  font-size: 1.2rem;
}

.account-detail-item__field-value {
  font-size: .9rem;
  color: var(--gray-light-color);
}

.item-edit {
  display: flex;
  align-items: center;
  cursor: pointer;
}

.edit-icon {
  width: .8rem;
  height: .8rem;
  margin-right: .5rem;
}

.account-detail-item__edit {
  font-size: .9rem;
  color: #000;
}

.modal {
  text-align: center;
}

.modal h1 {
  font-size: 1.3rem;
}

::v-deep(.modal-footer) {
  justify-content: space-around;
}

@media screen and (max-width: 460px) {
  .account-detail-item__edit {
    display: none;
  }
}
</style>