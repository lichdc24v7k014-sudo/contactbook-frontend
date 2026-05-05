<template>
  <div class="page">
    <!-- Thanh tìm kiếm -->
    <div class="row">
      <div class="col-md-10">
        <InputSearch v-model="searchText" @submit="refreshList" />
      </div>
    </div>

    <h4 class="mt-4"><i class="fas fa-address-book"></i> Danh bạ</h4>

    <div class="row">
      <!-- Danh sách liên hệ bên trái -->
      <div class="col-md-6">
        <ContactList
          v-if="filteredContactsCount > 0"
          :contacts="filteredContacts"
          v-model:activeIndex="activeIndex"
        />
        <p v-else class="text-muted">Không có liên hệ nào.</p>
      </div>

      <!-- Chi tiết liên hệ bên phải -->
      <div class="col-md-6" v-if="activeContact">
        <h5><i class="fas fa-address-card"></i> Chi tiết Liên hệ</h5>
        <ContactCard :contact="activeContact" />

        <!-- Thêm nút Hiệu chỉnh -->
        <router-link
          :to="{
            name: 'contact.edit',
            params: { id: activeContact._id },
          }"
          class="btn btn-warning btn-sm mt-2"
        >
          <i class="fas fa-edit"></i> Hiệu chỉnh
        </router-link>
      </div>
    </div>

    <!-- Các nút chức năng -->
    <div class="mt-4">
      <button class="btn btn-primary me-2" @click="refreshList">
        <i class="fas fa-sync"></i> Làm mới danh sách
      </button>
      <router-link :to="{ name: 'contact.add' }" class="btn btn-success me-2">
        <i class="fas fa-plus"></i> Thêm mới
      </router-link>
      <button class="btn btn-danger" @click="removeAllContacts">
        <i class="fas fa-trash"></i> Xóa tất cả
      </button>
    </div>
  </div>
</template>

<script>
import InputSearch from "../components/InputSearch.vue";
import ContactList from "../components/ContactList.vue";
import ContactCard from "../components/ContactCard.vue";
import ContactService from "../services/contact.service";

export default {
  components: {
    InputSearch,
    ContactList,
    ContactCard,
  },
  data() {
    return {
      contacts: [],
      activeIndex: -1,
      searchText: "",
    };
  },
  computed: {
    contactStrings() {
      return this.contacts.map((contact) => {
        const { name, email, address, phone } = contact;
        return [name, email, address, phone].join("").toLowerCase();
      });
    },
    filteredContacts() {
      if (!this.searchText) return this.contacts;
      const term = this.searchText.toLowerCase();
      return this.contacts.filter((contact, index) =>
        this.contactStrings[index].includes(term),
      );
    },
    filteredContactsCount() {
      return this.filteredContacts.length;
    },
    activeContact() {
      if (this.activeIndex < 0) return null;
      return this.filteredContacts[this.activeIndex];
    },
  },
  methods: {
    async retrieveContacts() {
      try {
        this.contacts = await ContactService.getAll();
      } catch (error) {
        console.log(error);
      }
    },
    refreshList() {
      this.retrieveContacts();
      this.activeIndex = -1;
    },
    async removeAllContacts() {
      if (confirm("Bạn muốn xóa tất cả Liên hệ?")) {
        try {
          await ContactService.deleteAll();
          this.refreshList();
        } catch (error) {
          console.log(error);
        }
      }
    },
    goToAddContact() {
      this.$router.push({ name: "contact.add" });
    },
  },
  mounted() {
    this.refreshList();
  },
};
</script>

<style scoped>
.page {
  max-width: 900px;
  margin: auto;
}
</style>
