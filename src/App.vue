<template>
  <div>
    <div class="w-[80%] m-auto mt-4 h-[100vh]">
      <main class="main">
        <header class="d-flex justify-content-between mb-3">
          <h3>Nhân viên</h3>
          <button @click="handleShowAddEmployee" class="btn btn-primary">
            Thêm mới nhân viên
          </button>
        </header>
        <div class="d-flex align-items-center justify-content-end gap-2 mb-3">
          <input
            style="width: 350px"
            type="text"
            class="form-control"
            placeholder="Tìm kiếm theo email"
            v-model="valueSearch"
          />
          <i class="fa-solid fa-arrows-rotate" title="Refresh"></i>
        </div>
        <!-- Danh sách nhân viên -->
        <table class="table table-bordered table-hover table-striped">
          <thead>
            <tr>
              <th>STT</th>
              <th>Họ và tên</th>
              <th>Ngày sinh</th>
              <th>Email</th>
              <th>Địa chỉ</th>
              <th>Trạng thái</th>
              <th colspan="2">Chức năng</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in filteredEmployees" :key="index">
              <td>{{ index + 1 }}</td>
              <td>{{ item.name }}</td>
              <td>{{ item.birth }}</td>
              <td>{{ item.email }}</td>
              <td>{{ item.address }}</td>
              <td>
                <div
                  v-if="item.status"
                  style="display: flex; align-items: center; gap: 8px"
                >
                  <div class="status status-active"></div>
                  <span> Đang hoạt động</span>
                </div>
                <div
                  v-else
                  style="display: flex; align-items: center; gap: 8px"
                >
                  <div class="status status-stop"></div>
                  <span> Ngừng hoạt động</span>
                </div>
              </td>
              <td>
                <span @click="ModelStatus(item.id)" class="button button-block">
                  {{ item.status ? "Chặn" : "Mở khóa" }}
                </span>
              </td>
              <td>
                <span
                  @click="handleEditEmployee(item.id)"
                  class="button button-edit"
                  >Sửa</span
                >
              </td>
              <td>
                <span
                  @click="modelDeleteEmployee(item.id)"
                  class="button button-delete"
                  >Xóa</span
                >
              </td>
            </tr>
          </tbody>
        </table>
        <footer class="d-flex justify-content-end align-items-center gap-3">
          <select class="form-select">
            <option selected>Hiển thị 10 bản ghi trên trang</option>
            <option>Hiển thị 20 bản ghi trên trang</option>
            <option>Hiển thị 50 bản ghi trên trang</option>
            <option>Hiển thị 100 bản ghi trên trang</option>
          </select>
          <ul class="pagination">
            <li class="page-item">
              <a class="page-link" href="#">Previous</a>
            </li>
            <li class="page-item"><a class="page-link" href="#">1</a></li>
            <li class="page-item"><a class="page-link" href="#">2</a></li>
            <li class="page-item"><a class="page-link" href="#">3</a></li>
            <li class="page-item"><a class="page-link" href="#">Next</a></li>
          </ul>
        </footer>
      </main>
    </div>

    <!-- Form thêm mới nhân viên -->
    <div class="overlay" v-show="showAddEmployee">
      <form class="form">
        <div class="d-flex justify-content-between align-items-center">
          <h4>Thêm mới nhân viên</h4>
          <i @click="handleShowAddEmployee" class="fa-solid fa-xmark"></i>
        </div>
        <div>
          <label class="form-label" for="userName">Họ và tên</label>
          <input
            v-model="employee.name"
            id="userName"
            type="email"
            class="form-control"
          />
          <!-- <div class="form-text error">Họ và tên không được để trống.</div> -->
        </div>
        <div>
          <label class="form-label" for="dateOfBirth">Ngày sinh</label>
          <input
            v-model="employee.birth"
            id="dateOfBirth"
            type="date"
            class="form-control"
          />
        </div>
        <!-- <div class="form-text error">
          Ngày sinh không được lớn hơn ngày hiện tại.
        </div> -->
        <div>
          <label class="form-label" for="email">Email</label>
          <input
            v-model="employee.email"
            id="email"
            type="text"
            class="form-control"
          />
        </div>
        <!-- <div class="form-text error">Email không được để trống.</div> -->
        <div>
          <label class="form-label" for="address">Địa chỉ</label>
          <textarea
            v-model="employee.address"
            class="form-control"
            id="address"
            rows="3"
          ></textarea>
        </div>
        <div>
          <button @click="addEmployee" class="w-100 btn btn-primary">
            Thêm mới
          </button>
        </div>
      </form>
    </div>

    <!-- Edit employee form -->
    <div class="overlay" v-show="showEditEmployee">
      <form class="form">
        <div class="d-flex justify-content-between align-items-center">
          <h4>Chỉnh sửa nhân viên</h4>
          <i @click="showEditEmployee = false" class="fa-solid fa-xmark"></i>
        </div>
        <div>
          <label class="form-label" for="userName">Họ và tên</label>
          <input
            v-model="employee.name"
            id="userName"
            type="text"
            class="form-control"
          />
        </div>
        <div>
          <label class="form-label" for="dateOfBirth">Ngày sinh</label>
          <input
            v-model="employee.birth"
            id="dateOfBirth"
            type="date"
            class="form-control"
          />
        </div>
        <div>
          <label class="form-label" for="email">Email</label>
          <input
            v-model="employee.email"
            id="email"
            type="text"
            class="form-control"
          />
        </div>
        <div>
          <label class="form-label" for="address">Địa chỉ</label>
          <textarea
            v-model="employee.address"
            class="form-control"
            id="address"
            rows="3"
          ></textarea>
        </div>
        <div>
          <button @click="updateEmployee" class="w-100 btn btn-primary">
            Cập nhật
          </button>
        </div>
      </form>
    </div>

    <!-- Modal xác nhận chặn tài khoản -->
    <div class="overlay" v-show="showModelStatus">
      <div class="modal-custom">
        <div class="modal-title">
          <h4>Cảnh báo</h4>
          <i class="fa-solid fa-xmark"></i>
        </div>
        <div class="modal-body-custom">
          <span>Bạn có chắc chắn muốn chặn tài khoản này?</span>
        </div>
        <div class="modal-footer-custom">
          <button @click="ModelStatus" class="btn btn-light">Hủy</button>
          <button @click="changeStatus" class="btn btn-danger">Xác nhận</button>
        </div>
      </div>
    </div>

    <!-- Modal xác nhận xóa tài khoản -->
    <div class="overlay" v-show="showModelDelete">
      <div class="modal-custom">
        <div class="modal-title">
          <h4>Cảnh báo</h4>
          <i class="fa-solid fa-xmark"></i>
        </div>
        <div class="modal-body-custom">
          <span>Bạn có chắc chắn muốn xóa tài khoản này?</span>
        </div>
        <div class="modal-footer-custom">
          <button class="btn btn-light">Hủy</button>
          <button @click="deleteEmployee" class="btn btn-danger">
            Xác nhận
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { computed, ref } from "vue";

// Employee data model
const employee = ref({
  id: Math.floor(Math.random() * 10000),
  name: "",
  birth: "",
  email: "",
  address: "",
  status: true,
});

// Get employee list from localStorage
const employees = ref(JSON.parse(localStorage.getItem("employees") || "[]"));

// Save employee list to localStorage
const saveEmployee = () => {
  localStorage.setItem("employees", JSON.stringify(employees.value));
};

// Show add employee form
const showAddEmployee = ref(false);
const handleShowAddEmployee = () => {
  showAddEmployee.value = !showAddEmployee.value;
};

// Add Employee with validation
const addEmployee = () => {
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  const currentDate = new Date().toISOString().split("T")[0];

  if (!employee.value.name) {
    alert("Họ và tên không được để trống.");
  } else if (
    !employee.value.email ||
    !emailPattern.test(employee.value.email)
  ) {
    alert("Email không hợp lệ.");
  } else if (!employee.value.birth || employee.value.birth > currentDate) {
    alert("Ngày sinh không được lớn hơn ngày hiện tại.");
  } else {
    employees.value.push({ ...employee.value });
    saveEmployee();
    employee.value = {
      id: Math.floor(Math.random() * 10000),
      name: "",
      birth: "",
      email: "",
      address: "",
      status: true,
    };
    showAddEmployee.value = false;
  }
};

// Block/Unblock Employee
const showModelStatus = ref(false);
const getIdStatus = ref(0);

const ModelStatus = (id) => {
  getIdStatus.value = id;
  showModelStatus.value = !showModelStatus.value;
};

const changeStatus = () => {
  const employeeToChange = employees.value.find(
    (item) => item.id === getIdStatus.value
  );
  if (employeeToChange) {
    employeeToChange.status = !employeeToChange.status;
    saveEmployee();
  }
  ModelStatus(); // Close modal after changing status
};

// Edit Employee
const showEditEmployee = ref(false);
const editEmployeeId = ref(null);

const handleEditEmployee = (id) => {
  const employeeToEdit = employees.value.find((item) => item.id === id);
  if (employeeToEdit) {
    employee.value = { ...employeeToEdit }; // Populate form with employee data
    showEditEmployee.value = true;
    editEmployeeId.value = id;
  }
};

const updateEmployee = () => {
  const index = employees.value.findIndex(
    (item) => item.id === editEmployeeId.value
  );
  if (index !== -1) {
    employees.value.splice(index, 1, { ...employee.value });
    saveEmployee();
    resetEmployee();
    showEditEmployee.value = false;
  }
};

// Delete Employee

const showModelDelete = ref(false);

const getIdDelete = ref(0);

const modelDeleteEmployee = (id) => {
  getIdDelete.value = id;

  showModelDelete.value = !showModelDelete.value;
};

const deleteEmployee = () => {
  employees.value = employees.value.filter(
    (item) => item.id !== getIdDelete.value
  );
  saveEmployee();
  showModelDelete.value = !showModelDelete.value;
};

// Search email
const valueSearch = ref("");

const filteredEmployees = computed(() =>
  employees.value.filter((employee) =>
    employee.email.toLowerCase().includes(valueSearch.value.toLowerCase())
  )
);
</script>
<style scoped>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  font-size: 14px;
  font-family: sans-serif;
}

.main {
  /* height: 100vh; */
  margin: 50px 100px;
}

.fa-arrows-rotate {
  font-size: 20px;
  cursor: pointer;
  color: gray;
}

.fa-arrows-rotate:hover {
  color: rgb(184, 180, 180);
  transform: rotate(20deg);
  transition: all 0.5s linear;
}

.button {
  padding: 4px 12px;
  line-height: 30px;
  border-radius: 4px;
  color: #fff;
  cursor: pointer;
}

.button-edit {
  background-color: orange;
}

.button-delete {
  background-color: red;
}

.button-block {
  background-color: green;
}

td:first-child,
td:nth-child(6),
td:nth-child(7) {
  text-align: center;
}

.form-select {
  width: 270px !important;
}

.overlay {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.form {
  background-color: #fff;
  width: 500px;
  padding: 20px 24px;
  border-radius: 4px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.form-label {
  font-weight: 600;
  margin-bottom: 8px;
}

.fa-xmark {
  font-size: 20px;
  cursor: pointer;
}

.error {
  color: red !important;
}

.status-container {
  border: 1px solid #dadada;
  padding: 4px 8px;
  border-radius: 4px;
}

.status {
  height: 10px;
  width: 10px;
  border-radius: 50%;
  border: 1px solid transparent;
}

.status-active {
  background-color: green;
}

.status-stop {
  background-color: red;
}

.modal-custom {
  background-color: #fff;
  padding: 20px 24px;
  border-radius: 4px;
  width: 400px;
}

.modal-title {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.modal-body-custom {
  padding: 20px 0;
}

.modal-footer-custom {
  display: flex;
  justify-content: end;
  gap: 8px;
}

.pagination {
  margin-bottom: 0;
}
</style>
