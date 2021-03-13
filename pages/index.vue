<template>
  <div>
    <nav class="w-full h-16 bg-blue-700 flex justify-between">
      <div class="inline-block align-middle text-white text-2xl px-4 py-2 m-2">
        <label>Manage</label> <label class="font-bold">Employees</label>
        <div
          class="inline-block text-xl text-white absolute top-0 right-0 mt-1 px-4 py-2 m-2"
        >
          <div class="p-1 bg-red-600 rounded-sm inline-block">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 2 20 20"
              fill="currentColor"
              class="h-5 w-5 text-white inline-block"
            >
              <path
                fill-rule="evenodd"
                d="M10 18a8 8 0 100-16 8 8 0 000 16zM7 9a1 1 0 000 2h6a1 1 0 100-2H7z"
                clip-rule="evenodd"
              />
            </svg>
            <label class="inline-block">Delete</label>
          </div>

          <div
            class="p-1 bg-green-500 rounded-sm inline-block"
            @click="
              () => {
                showModal = true
                titleModal = 'Add Employee'
              }
            "
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 2 20 20"
              fill="currentColor"
              class="h-5 w-5 text-white inline-block"
            >
              <path
                fill-rule="evenodd"
                d="M10 18a8 8 0 100-16 8 8 0 000 16zm1-11a1 1 0 10-2 0v2H7a1 1 0 100 2h2v2a1 1 0 102 0v-2h2a1 1 0 100-2h-2V7z"
                clip-rule="evenodd"
              />
            </svg>
            <label class="inline-block">Add New Employee</label>
          </div>
        </div>
      </div>
    </nav>

    <div class="mt-5 container mx-auto">
      <table class="table-auto w-full text-left">
        <thead>
          <tr>
            <th class="px-4 py-2">
              <input type="checkbox" @click="selectAll" v-model="selected" />
            </th>
            <th class="px-4 py-2">Name</th>
            <th class="px-4 py-2">Email</th>
            <th class="px-4 py-2">Address</th>
            <th class="px-4 py-2">Phone</th>
            <th class="px-4 py-2">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(employee, idx) in employees" :key="idx">
            <td class="border px-4 py-2">
              <input
                type="checkbox"
                v-model="employeeIds"
                :value="employee.id"
                number
              />
            </td>
            <td class="border px-4 py-2">
              {{ employee.fristName }} {{ employee.lastName }}
            </td>
            <td class="border px-4 py-2">{{ employee.email }}</td>
            <td class="border px-4 py-2">{{ employee.address }}</td>
            <td class="border px-4 py-2">{{ employee.phone }}</td>
            <td class="border px-4 py-2">
              <div>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                  class="h-5 w-5 text-yellow-500 mr-3 inline-block"
                  @click="editEmployee(idx)"
                >
                  <path
                    d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z"
                  />
                </svg>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                  class="h-5 w-5 text-red-600 inline-block"
                  @click="deleteEmployee(idx)"
                >
                  <path
                    fill-rule="evenodd"
                    d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                    clip-rule="evenodd"
                  />
                </svg>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <Modal
      :show="showModal"
      @close="
        () => {
          showModal = false
          clear()
        }
      "
      :name-header="titleModal"
      name-button="Apply"
      @apply="applyEmployee()"
      class="flex justify-between"
    >
      <div class="form px-5">
        <div>
          <label>Fristname</label
          ><input
            class="w-full"
            type="text"
            placeholder="Fristname"
            v-model="fristName"
          />
        </div>
        <div>
          <label>Lastname</label
          ><input
            class="w-full"
            type="text"
            placeholder="Lastname"
            v-model="lastName"
          />
        </div>
        <div>
          <label>Department</label>
          <select
            name="Department"
            class="w-full"
            v-model="department"
            required
          >
            <option value=""></option>
            <option value="back-end developer">back-end developer</option>
            <option value="application programmer">
              application programmer
            </option>
            <option value="full-stack develorer">full-stack develorer</option>
            <option value="front-end develorer">front-end develorer</option>
          </select>
        </div>
        <div>
          <label>Email</label
          ><input
            class="w-full"
            type="email"
            placeholder="Email"
            v-model="email"
          />
        </div>
        <div>
          <label>Address</label
          ><input
            class="w-full"
            type="text"
            placeholder="Address"
            v-model="address"
          />
        </div>
        <div>
          <label>Phone</label
          ><input
            class="w-full"
            type="text"
            placeholder="Phone"
            v-model="phone"
          />
        </div>
      </div>
    </Modal>
  </div>
</template>

<script>
import Modal from '@/components/chared/Modal'
export default {
  components: { Modal },

  data() {
    return {
      showModal: false,
      fristName: '',
      lastName: '',
      department: '',
      email: '',
      address: '',
      phone: '',
      titleModal: '',
      employee: {},
      nowIdx: 0,
      employees: [
        {
          id: 1,
          fristName: 'Thomas',
          lastName: 'Hardy',
          email: 'thomashady@mail.com',
          address: '89 Chiaroscuro Rd, Portland, USA',
          phone: '(171)555-2222',
          department: 'back-end developer',
        },
        {
          id: 2,
          fristName: 'Dominique',
          lastName: 'Perrier',
          email: 'dominiqueperrier@mail.com',
          address: 'obere Str.57, Berlin, Germany',
          phone: '(313)555-5735',
          department: 'application programmer',
        },
        {
          id: 3,
          fristName: 'Maria',
          lastName: 'Anders',
          email: 'mariaanders@mail.com',
          address: '25, rue Lauriston, Paris, France',
          phone: '(503)555-9935',
          department: 'full-stack develorer',
        },
        {
          id: 4,
          fristName: 'Fran',
          lastName: 'Wilson',
          email: 'franwilson@mail.com',
          address: 'C/ Araquil, 67, Madrid, Spain',
          phone: '(204)555-5731',
          department: 'full-stack develorer',
        },
        {
          id: 2,
          fristName: 'Dominique',
          lastName: 'Perrier',
          email: 'dominiqueperrier@mail.com',
          address: 'obere Str.57, Berlin, Germany',
          phone: '(480)555-5735',
          department: 'front-end develorer',
        },
      ],
      selecteded: [],
      employeeIds: [],
      selected: false,
    }
  },

  methods: {
    deleteEmployee(idx) {
      this.employees.splice(idx, 1)
    },
    selectAll() {
      this.employeeIds = []
      if (!this.selected) {
        for (this.employee in this.employees) {
          this.employeeIds.push(this.employees[this.employee].id)
        }
      }
    },
    applyEmployee(idx) {
      if (this.department === '') {
        alert('กรุณาเลือกแผนก')
        return false
      } else if (this.fristName === '') {
        alert('กรุณากรอกชื่อ')
        return false
      } else if (this.lastName === '') {
        alert('กรุณากรอกนามสกุล')
        return false
      } else if (this.email === '') {
        alert('กรุณากรอกอีเมล')
        return false
      } else if (this.address === '') {
        alert('กรุณากรอกที่อยู่')
        return false
      } else if (this.phone === '') {
        alert('กรุณากรอกเบอร์โทร')
        return false
      }
      this.showModal = false
      const employee = {
        fristName: this.fristName,
        lastName: this.lastName,
        email: this.email,
        address: this.address,
        phone: this.phone,
        department: this.department,
      }

      if (this.titleModal === 'Add Employee') {
        this.employees.push(employee)
      } else if (this.titleModal === 'Edit Employee') {
        this.employees[this.nowIdx] = employee
      }
      this.clear()
    },
    editEmployee(idx) {
      this.nowIdx = idx
      this.titleModal = 'Edit Employee'
      this.fristName = this.employees[idx].fristName
      this.lastName = this.employees[idx].lastName
      this.department = this.employees[idx].department
      this.email = this.employees[idx].email
      this.address = this.employees[idx].address
      this.phone = this.employees[idx].phone
      // console.log(this.employees[idx])
      this.showModal = true
    },

    clear() {
      this.fristName = ''
      this.lastName = ''
      this.department = ''
      this.email = ''
      this.address = ''
      this.phone = ''
    },
  },
}
</script>

<style>
table tr:nth-child(even) {
  background-color: #f2f2f2;
  border: #f2f2f2;
}
.form input {
  @apply bg-gray-200;
  @apply border-transparent;
  @apply block;
  @apply p-2;
  @apply mb-3;
}
.form input:focus {
  @apply bg-white;
  @apply border-blue-400;
}
.form select:focus {
  @apply bg-white;
  @apply border-blue-400;
}
.form select {
  @apply bg-gray-200;
  @apply border-transparent;
  @apply block;
  @apply p-2;
  @apply mb-3;
}
</style>
