<template>
  <div class="main-wrapper">
    <div class="container-fluid py-4">
      <div class="mb-4 text-center">
        <div class="btn-group">
          <button @click="currentSection = 'home'" :class="['btn', currentSection === 'home' ? 'btn-primary active' : 'btn-outline-primary']"><i class="bi bi-house-door-fill me-2"></i>Home</button>
          <button @click="currentSection = 'profile'" :class="['btn', currentSection === 'profile' ? 'btn-primary active' : 'btn-outline-primary']"><i class="bi bi-person-circle me-2"></i>Profile</button>
          <button @click="currentSection = 'courses'" :class="['btn', currentSection === 'courses' ? 'btn-primary active' : 'btn-outline-primary']"><i class="bi bi-list-ul me-2"></i>Courses</button>
          <button @click="currentSection = 'editCourses'" :class="['btn', currentSection === 'editCourses' ? 'btn-success active' : 'btn-outline-success']"><i class="bi bi-pencil-square me-2"></i>Edit Courses</button>
          <button @click="currentSection = 'addCourses'" :class="['btn', currentSection === 'addCourses' ? 'btn-danger active' : 'btn-outline-danger']"><i class="bi bi-plus-circle-fill me-2"></i>Add Courses</button>
        </div>
      </div>

      <transition name="fade" mode="out-in">

        <div v-if="currentSection === 'home'" class="content-section card mb-4 home-view" key="home-view">
           <div class="card-header">
             <h2><i class="bi bi-house-heart-fill me-2 text-primary"></i>Welcome Home</h2>
           </div>
           <div class="card-body p-0">
             <div class="row g-0 align-items-stretch">
               <div class="col-md-5 col-lg-4 home-image-col">
                 <img src="/home.jpg"
                      alt="Welcome Image"
                      class="welcome-image-fill"> </div>
               <div class="col-md-7 col-lg-8 home-text-col d-flex flex-column justify-content-center">
                 <div class="p-4 p-lg-5 text-center text-md-start">
                   <h3 class="display-6">สวัสดีครับ ผม</h3>
                   <h1 class="display-5 fw-bold mb-3 text-primary">{{ profile.student_name }}</h1>
                   <p class="lead">ยินดีต้อนรับสู่โปรเจ็คของผม</p>
                   <p>คุณสามารถดูโปรไฟล์, ดูวิชาที่ลงทะเบียน, จัดการรายวิชา และเพิ่มรายวิชาใหม่ได้จากเมนูด้านบน</p>
                 </div>
               </div>
             </div>
           </div>
         </div>

        <div v-else-if="currentSection === 'profile' && !editProfileMode" class="content-section card mb-4 profile-view large-profile" key="profile-view">
           <div class="card-header d-flex justify-content-between align-items-center">
             <h2><i class="bi bi-person-badge me-2"></i>Profile</h2>
             <button class="btn btn-sm btn-outline-primary" @click="editProfile">
               <i class="bi bi-pencil-fill me-1"></i>Edit Profile
             </button>
           </div>
           <div class="card-body p-md-5">
             <div class="row align-items-center">
               <div class="col-lg-4 col-md-5 text-center mb-4 mb-md-0 profile-image-col">
                 <img src="/485092743_645623608206005_3191432283257189807_n.jpg"
                      alt="Profile Picture"
                      class="img-fluid rounded-circle profile-pic-display-large shadow-lg">
               </div>
               <div class="col-lg-8 col-md-7">
                 <ul class="list-group list-group-flush profile-info-list">
                   <li class="list-group-item d-flex align-items-start profile-list-item">
                     <i class="bi bi-person-vcard me-3 profile-list-icon text-primary"></i>
                     <div class="flex-grow-1">
                       <strong class="profile-info-label d-block mb-0">ชื่อนิสิต:</strong>
                       <span class="profile-info-value fs-5">{{ profile.student_name }}</span>
                     </div>
                   </li>
                   <li class="list-group-item d-flex align-items-start profile-list-item">
                     <i class="bi bi-hash me-3 profile-list-icon text-secondary"></i>
                       <div class="flex-grow-1">
                        <strong class="profile-info-label d-block mb-0">รหัสนิสิต:</strong>
                        <span class="profile-info-value fs-5">{{ profile.student_id }}</span>
                      </div>
                   </li>
                   <li class="list-group-item d-flex align-items-start profile-list-item">
                     <i class="bi bi-mortarboard-fill me-3 profile-list-icon text-success"></i>
                       <div class="flex-grow-1">
                        <strong class="profile-info-label d-block mb-0">สาขา:</strong>
                        <span class="profile-info-value fs-5">{{ profile.department }}</span>
                      </div>
                   </li>
                   <li class="list-group-item d-flex align-items-start profile-list-item">
                       <i class="bi bi-building me-3 profile-list-icon text-info"></i>
                       <div class="flex-grow-1">
                        <strong class="profile-info-label d-block mb-0">โรงเรียนเดิม:</strong>
                        <span class="profile-info-value fs-5">{{ profile.school }}</span>
                      </div>
                   </li>
                 </ul>
                 </div>
             </div>
           </div>
         </div>

        <div v-else-if="currentSection === 'profile' && editProfileMode" class="content-section card mb-4" key="profile-edit">
          <div class="card-header">
           <h2><i class="bi bi-pencil-square me-2"></i>Edit Profile</h2>
          </div>
          <div class="card-body">
            <form @submit.prevent="saveProfile">
              <div class="row mb-3 align-items-center">
                <label for="editStudentName" class="col-sm-3 col-form-label text-sm-end"><strong>ชื่อนิสิต:</strong></label>
                <div class="col-sm-9">
                  <input type="text" id="editStudentName" class="form-control" v-model="editedProfile.student_name">
                </div>
              </div>
              <div class="row mb-3 align-items-center">
                <label for="editStudentId" class="col-sm-3 col-form-label text-sm-end"><strong>รหัสนิสิต:</strong></label>
                <div class="col-sm-9">
                  <input type="text" id="editStudentId" class="form-control" v-model="editedProfile.student_id" pattern="\d{10}" title="กรอกรหัสนิสิตเป็นตัวเลข 10 หลัก">
                </div>
              </div>
              <div class="row mb-3 align-items-center">
                <label for="editDepartment" class="col-sm-3 col-form-label text-sm-end"><strong>สาขา:</strong></label>
                <div class="col-sm-9">
                  <input type="text" id="editDepartment" class="form-control" v-model="editedProfile.department">
                </div>
              </div>
              <div class="row mb-3 align-items-center">
                <label for="editSchool" class="col-sm-3 col-form-label text-sm-end"><strong>โรงเรียนเดิม:</strong></label>
                <div class="col-sm-9">
                  <input type="text" id="editSchool" class="form-control" v-model="editedProfile.school">
                </div>
              </div>
              <div class="d-flex justify-content-end mt-4">
                <button type="button" @click="cancelEditProfile" class="btn btn-secondary me-2">
                  <i class="bi bi-x-circle me-1"></i>Cancel
                </button>
                <button type="submit" class="btn btn-primary">
                  <i class="bi bi-save-fill me-1"></i>Save Profile
                </button>
              </div>
            </form>
          </div>
        </div>

        <div v-else-if="currentSection === 'courses'" class="content-section card mb-4 courses-view" key="courses">
          <div class="card-header">
            <h2><i class="bi bi-book-half me-2"></i>Courses</h2>
          </div>
          <div class="card-body table-card-body">
            <div class="mb-3 w-100" style="max-width: 400px;">
              <label for="semesterSelector" class="form-label">Select Semester:</label>
              <select id="semesterSelector" class="form-select" v-model="selectedSemester">
                <option v-for="option in semesterOptions" :key="option.value" :value="option.value">
                    {{ option.text }}
                </option>
                <option v-if="!semesterOptions.length && defaultSemesterKey" :value="defaultSemesterKey">
                    {{ formatSemesterDisplay(defaultSemesterKey) }}
                 </option>
              </select>
            </div>
            <div class="table-responsive">
              <table class="table table-bordered table-striped table-hover">
                <thead class="table-light text-center">
                  <tr>
                    <th>รหัสวิชา</th>
                    <th>ชื่อวิชา</th>
                    <th>หน่วยกิต</th>
                    <th>เกรด</th>
                  </tr>
                </thead>
                <tbody class="text-center">
                  <tr v-if="!courses[selectedSemester] || courses[selectedSemester].length === 0">
                    <td colspan="4" class="text-muted fst-italic">No courses found for this semester.</td>
                  </tr>
                  <tr v-else v-for="course in courses[selectedSemester]" :key="course.id">
                   <td>{{ course.id }}</td>
                   <td class="text-start">{{ course.name }}</td>
                   <td>{{ course.credits }}</td>
                   <td>{{ course.grade }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div v-if="courses[selectedSemester] && courses[selectedSemester].length > 0" class="mt-3 bg-light p-3 rounded text-center gpa-box">
              <p class="fw-bold mb-1">GPA: {{ calculateGPA(selectedSemester).gpa }}</p>
              <p class="fw-bold mb-0">หน่วยกิตรวม (ที่คำนวณ GPA): {{ calculateGPA(selectedSemester).totalCredits }}</p>
            </div>
          </div>
        </div>

        <div v-else-if="currentSection === 'editCourses'" class="content-section card mb-4 edit-courses-view" key="edit-courses">
           <div class="card-header">
             <h2><i class="bi bi-pencil-square me-2 text-success"></i>Edit Courses</h2>
           </div>
           <div class="card-body table-card-body">
             <div class="mb-3 w-100" style="max-width: 400px;">
               <label for="semesterSelectorEdit" class="form-label">เลือกภาคการเรียน:</label>
               <select id="semesterSelectorEdit" class="form-select" v-model="selectedEditSemester">
                  <option v-for="option in semesterOptions" :key="option.value" :value="option.value">
                      {{ option.text }}
                  </option>
                   <option v-if="!semesterOptions.length && defaultSemesterKey" :value="defaultSemesterKey">
                      {{ formatSemesterDisplay(defaultSemesterKey) }}
                   </option>
               </select>
             </div>
             <div class="table-responsive">
               <table class="table table-bordered table-striped table-hover">
                 <thead class="table-light text-center">
                   <tr>
                     <th>รหัสวิชา</th>
                     <th>ชื่อวิชา</th>
                     <th>หน่วยกิต</th>
                     <th>เกรด</th>
                     <th>จัดการ</th>
                   </tr>
                 </thead>
                 <tbody class="text-center">
                   <tr v-if="!courses[selectedEditSemester] || courses[selectedEditSemester].length === 0">
                     <td colspan="5" class="text-muted fst-italic">No courses found for this semester.</td>
                   </tr>
                   <tr v-else v-for="course in courses[selectedEditSemester]" :key="course.id">
                    <td>{{ course.id }}</td>
                    <td class="text-start">{{ course.name }}</td>
                    <td>{{ course.credits }}</td>
                    <td>{{ course.grade }}</td>
                    <td>
                      <button class="btn btn-warning btn-sm me-2" @click="showEditForm(course)">
                        <i class="bi bi-pencil-fill"></i> <span class="d-none d-md-inline">Edit</span>
                      </button>
                      <button class="btn btn-danger btn-sm" @click="deleteCourse(course.id, selectedEditSemester)">
                        <i class="bi bi-trash-fill"></i> <span class="d-none d-md-inline">Delete</span>
                      </button>
                    </td>
                   </tr>
                 </tbody>
               </table>
             </div>

             <transition name="slide-fade">
               <div v-if="editingCourse" class="mt-4 card border-warning w-100 mx-auto edit-course-form-container" style="max-width: 600px;">
                 <div class="card-header bg-warning-subtle">
                   <h3 class="mb-0"><i class="bi bi-pencil me-2"></i>แก้ไขวิชา: <span class="fw-bold">{{ editingCourse.id }}</span></h3>
                 </div>
                 <div class="card-body">
                   <div class="mb-3">
                     <label for="editCourseId" class="form-label">รหัสวิชา (ID):</label>
                     <input type="text" id="editCourseId" class="form-control"
                            v-model.trim="editedCourse.id"
                            pattern="\d{8}"
                            title="กรอกรหัสวิชาเป็นตัวเลข 8 หลัก"
                            required>
                   </div>
                   <div class="mb-3">
                     <label for="editCourseName" class="form-label">ชื่อวิชา:</label>
                     <input type="text" id="editCourseName" class="form-control" v-model.trim="editedCourse.name" required>
                   </div>
                   <div class="mb-3">
                     <label for="editCredits" class="form-label">หน่วยกิต:</label>
                     <input type="number" id="editCredits" class="form-control" v-model.number="editedCourse.credits" min="1" required>
                   </div>
                   <div class="mb-3">
                     <label for="editGrade" class="form-label">เกรด:</label>
                     <select id="editGrade" class="form-select" v-model="editedCourse.grade" required>
                        <option value="" disabled>เลือกเกรด</option>
                        <option value="A">A</option>
                        <option value="B+">B+</option>
                        <option value="B">B</option>
                        <option value="C+">C+</option>
                        <option value="C">C</option>
                        <option value="D+">D+</option>
                        <option value="D">D</option>
                        <option value="F">F</option>
                        <option value="N">N</option>
                        <option value="S">S</option>
                        <option value="U">U</option>
                        <option value="W">W</option>
                        <option value="I">I</option>
                     </select>
                   </div>
                   <div class="d-flex justify-content-end">
                     <button type="button" @click="cancelEdit" class="btn btn-secondary me-2">
                       <i class="bi bi-x-circle me-1"></i>ยกเลิก
                     </button>
                     <button type="button" @click="saveEditedCourse" class="btn btn-primary">
                       <i class="bi bi-save-fill me-1"></i>บันทึกการแก้ไข
                     </button>
                   </div>
                 </div>
               </div>
             </transition>
           </div>
         </div>

        <div v-else-if="currentSection === 'addCourses'" class="content-section card mb-4 add-courses-view" key="add-courses">
        <div class="card-header">
          <h2><i class="bi bi-plus-square-dotted me-2 text-danger"></i>Add Courses</h2>
        </div>
        <div class="card-body">
          <form @submit.prevent="addCourse">
            <div class="row g-4 justify-content-center align-items-end">
              <div class="col-md-6 col-lg-3 mb-3">
                <label for="semesterSelectorAdd" class="form-label">เลือกภาคการเรียน:</label>
                <select id="semesterSelectorAdd" class="form-select" v-model="newCourse.semester">
                    <option v-for="option in semesterOptions" :key="option.value" :value="option.value">
                        {{ option.text }}
                    </option>
                    <option v-if="!semesterOptions.length && defaultSemesterKey" :value="defaultSemesterKey">
                      {{ formatSemesterDisplay(defaultSemesterKey) }}
                    </option>
                </select>
              </div>
              <div class="col-md-6 col-lg-2 mb-3">
                 <label for="courseId" class="form-label">รหัสวิชา (ID):</label>
                <input type="text" id="courseId" class="form-control" placeholder="8 หลัก" v-model.trim="newCourse.id" pattern="\d{8}" title="กรอกรหัสวิชาเป็นตัวเลข 8 หลัก" required>
              </div>
              <div class="col-md-12 col-lg-4 mb-3">
                <label for="courseName" class="form-label">ชื่อวิชา:</label>
                <input type="text" id="courseName" class="form-control" placeholder="กรอกชื่อวิชา" v-model.trim="newCourse.name" required>
              </div>
              <div class="col-md-6 col-lg-1 mb-3">
                <label for="credits" class="form-label">หน่วยกิต:</label>
                <input type="number" id="credits" class="form-control" placeholder="> 0" v-model.number="newCourse.credits" min="1" required>
              </div>
              <div class="col-md-6 col-lg-2 mb-3">
                <label for="grade" class="form-label">เกรด:</label>
                <select id="grade" class="form-select" v-model="newCourse.grade" required>
                    <option value="" disabled>เลือกเกรด</option>
                    <option value="A">A</option>
                    <option value="B+">B+</option>
                    <option value="B">B</option>
                    <option value="C+">C+</option>
                    <option value="C">C</option>
                    <option value="D+">D+</option>
                    <option value="D">D</option>
                    <option value="F">F</option>
                    <option value="N">N</option>
                    <option value="S">S</option>
                    <option value="U">U</option>
                    <option value="W">W</option>
                    <option value="I">I</option>
                </select>
              </div>
              <div class="col-12 col-lg-auto mb-3 align-self-lg-end d-grid d-lg-block">
                  <button type="submit" class="btn btn-danger">
                    <i class="bi bi-plus-lg me-1"></i>เพิ่มวิชา
                  </button>
              </div>
            </div>
          </form>
        </div>
      </div>

      </transition>
    </div>
  </div>
</template>

<script>
const API_BASE_URL = "http://localhost:3000";
// Define semester keys consistently
const SEMESTER_KEYS = ['2566-1', '2566-2', '2567-1', '2567-2'];
const LATEST_SEMESTER_KEY = '2567-2'; // Or determine dynamically

export default {
  name: 'StudentCourses',
  data() {
    return {
      currentSection: 'home',
      editProfileMode: false,
      selectedSemester: LATEST_SEMESTER_KEY,
      selectedEditSemester: LATEST_SEMESTER_KEY,
      editingCourse: null, // Holds the original course object being edited
      editedCourse: { id: '', name: '', credits: null, grade: '' }, // Holds the edited values
      profile: {
        student_name: "",
        student_id: "",
        department: "",
        school: "",
      },
      editedProfile: { student_name: "", student_id: "", department: "", school: "" },
      newCourse: { semester: LATEST_SEMESTER_KEY, id: '', name: '', credits: null, grade: '' },
      courses: {}, // Stores courses keyed by semester string, e.g., courses['2566-1'] = [...]
      isLoading: {
        profile: true,
        courses: true,
      }
    };
  },
  computed: {
      semesterOptions() {
          return SEMESTER_KEYS.map(key => ({
              value: key,
              text: this.formatSemesterDisplay(key)
          })).sort((a, b) => a.value.localeCompare(b.value));
      },
      defaultSemesterKey() {
          return LATEST_SEMESTER_KEY;
      }
  },
  methods: {
    editProfile() {
      this.editedProfile = { ...this.profile };
      this.editProfileMode = true;
    },
    async saveProfile() {
      if (!this.editedProfile.student_name || !this.editedProfile.student_id ||
          !this.editedProfile.department || !this.editedProfile.school) {
        alert("กรุณากรอกข้อมูลโปรไฟล์ให้ครบทุกช่อง");
        return;
      }
      if (!/^\d{10}$/.test(this.editedProfile.student_id)) {
          alert("รูปแบบรหัสนิสิตไม่ถูกต้อง (ต้องเป็นตัวเลข 10 หลัก)");
          return;
      }

      this.isLoading.profile = true;
      try {
          const res = await fetch(`${API_BASE_URL}/students`, {
            method: "PUT",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(this.editedProfile),
          });

          if (!res.ok) {
              throw new Error(`HTTP error! status: ${res.status}`);
          }
          this.profile = { ...this.editedProfile };
          this.editProfileMode = false;
          alert("บันทึกข้อมูลโปรไฟล์เรียบร้อยแล้ว");

      } catch (error) {
          console.error("Error saving profile:", error);
          alert("เกิดข้อผิดพลาดในการบันทึกโปรไฟล์ กรุณาลองใหม่อีกครั้ง");
      } finally {
          this.isLoading.profile = false;
      }
    },
    cancelEditProfile() {
      this.editProfileMode = false;
    },
    getGradePoints(grade) {
      const gradePoints = {
        'A': 4.0, 'B+': 3.5, 'B': 3.0, 'C+': 2.5, 'C': 2.0,
        'D+': 1.5, 'D': 1.0, 'F': 0.0
      };
      return gradePoints[grade] ?? null;
    },
    calculateGPA(semesterKey) {
        const semesterCourses = this.courses[semesterKey];
        if (!semesterCourses || semesterCourses.length === 0) {
            return { totalCredits: 0, gpa: 'N/A' };
        }
        let totalPoints = 0;
        let calculableCredits = 0;
        semesterCourses.forEach(course => {
            const pointsValue = this.getGradePoints(course.grade);
            if (pointsValue !== null && typeof course.credits === 'number' && course.credits > 0) {
                calculableCredits += course.credits;
                totalPoints += (pointsValue * course.credits);
            }
        });
        const gpa = calculableCredits > 0 ? (totalPoints / calculableCredits) : 0;
        return {
            totalCredits: calculableCredits,
            gpa: calculableCredits > 0 ? gpa.toFixed(2) : 'N/A'
        };
    },
    formatSemesterDisplay(semesterKey) {
        if (!semesterKey || typeof semesterKey !== 'string') return 'Invalid Semester';
        const parts = semesterKey.split('-');
        if (parts.length === 2) {
          const year = parts[0];
          const termNumber = parts[1];
          let termText = `ภาคเรียนที่ ${termNumber}`;
          if (termNumber === '1') termText = 'ภาคเรียนที่ 1';
          else if (termNumber === '2') termText = 'ภาคเรียนที่ 2';
          return `ปีการศึกษา ${year} ${termText}`;
        }
        return semesterKey;
    },
    showEditForm(course) {
      this.editingCourse = course;
      this.editedCourse = { ...course };
    },
    async saveEditedCourse() {
      const originalCourseId = this.editingCourse.id; // ID เดิมตอนกด Edit
      const semesterKey = this.selectedEditSemester;
      const isIdChanged = this.editedCourse.id !== originalCourseId; // เช็คว่า ID เปลี่ยนหรือไม่

      // --- Validation ---
      // 1. Basic field validation
      if (!this.editedCourse.id || !this.editedCourse.name || !this.editedCourse.credits || this.editedCourse.credits <= 0 || !this.editedCourse.grade) {
        alert("กรุณากรอกข้อมูลวิชาให้ครบถ้วนและถูกต้อง (หน่วยกิตต้องเป็นเลขบวก, เลือกเกรด)");
        return;
      }
      // 2. ID Format validation
      if (!/^\d{8}$/.test(this.editedCourse.id)) {
          alert("รูปแบบรหัสวิชา (ID) ไม่ถูกต้อง (ต้องเป็นตัวเลข 8 หลัก)");
          return;
      }
      // 3. ID Uniqueness Validation (Only if ID has changed)
      if (isIdChanged) {
          const existingCourseWithNewId = this.courses[semesterKey]?.find(
              course => course.id === this.editedCourse.id
          );
          if (existingCourseWithNewId) {
              alert(`รหัสวิชา (ID) '${this.editedCourse.id}' นี้มีอยู่แล้วในภาคเรียนนี้ กรุณาใช้รหัสอื่น`);
              return;
          }
      }
      // --- End Validation ---

      if (!semesterKey || !originalCourseId) {
          console.error("Error: Missing semesterKey or original courseId.");
          alert("เกิดข้อผิดพลาด: ไม่สามารถระบุภาคเรียนหรือรหัสวิชาเดิมได้");
          return;
      }

      // --- Logic based on whether ID changed ---
      try {
          if (isIdChanged) {
              // === ID HAS CHANGED: Perform DELETE + POST ===

              // 1. DELETE the old course
              const deleteRes = await fetch(`${API_BASE_URL}/${semesterKey}/${originalCourseId}`, {
                  method: 'DELETE',
              });
              if (!deleteRes.ok) {
                  throw new Error(`ลบข้อมูลเดิม (ID: ${originalCourseId}) ไม่สำเร็จ: ${deleteRes.status}`);
              }

              // 2. POST the new course data (with the new ID)
              //    Make sure editedCourse contains all necessary fields including the new ID
              const postRes = await fetch(`${API_BASE_URL}/${semesterKey}`, {
                  method: 'POST',
                  headers: { 'Content-Type': 'application/json' },
                  body: JSON.stringify(this.editedCourse), // Send the object with the new ID
              });
              if (!postRes.ok) {
                   // Attempt to rollback or inform user? For now, just throw error.
                   // If POST fails after DELETE, data might be lost. More robust handling needed for production.
                   if (postRes.status === 409) { // Example: Conflict if ID somehow exists again
                        throw new Error(`สร้างข้อมูลใหม่ (ID: ${this.editedCourse.id}) ไม่สำเร็จ: รหัสวิชาอาจมีอยู่แล้ว`);
                   } else {
                        throw new Error(`สร้างข้อมูลใหม่ (ID: ${this.editedCourse.id}) ไม่สำเร็จ: ${postRes.status}`);
                   }
              }

              // 3. Update Local State (Refetching is simplest)
              await this.fetchCoursesForSemester(semesterKey); // Reload data for the semester
              alert(`แก้ไขและเปลี่ยนรหัสวิชาเป็น '${this.editedCourse.id}' เรียบร้อย!`);

          } else {
              // === ID DID NOT CHANGE: Perform regular PUT ===
              const putRes = await fetch(`${API_BASE_URL}/${semesterKey}/${originalCourseId}`, {
                  method: "PUT",
                  headers: { "Content-Type": "application/json" },
                  body: JSON.stringify(this.editedCourse),
              });

              if (!putRes.ok) {
                 throw new Error(`แก้ไขข้อมูล (ID: ${originalCourseId}) ไม่สำเร็จ: ${putRes.status}`);
              }

              // Update local state directly for non-ID changes
              const courseIndex = this.courses[semesterKey]?.findIndex(c => c.id === originalCourseId);
              if (courseIndex !== -1) {
                 Object.assign(this.courses[semesterKey][courseIndex], this.editedCourse);
                 alert(`แก้ไขวิชา (ID: ${this.editedCourse.id}) เรียบร้อย!`);
              } else {
                 console.error("Course not found locally after successful PUT?");
                 // If not found, refetching might be safer
                 await this.fetchCoursesForSemester(semesterKey);
                 alert(`แก้ไขวิชา (ID: ${this.editedCourse.id}) เรียบร้อย! (รีเฟรชข้อมูล)`);
              }
          }

          this.cancelEdit(); // Close form only on full success

      } catch (error) {
          console.error("Error saving course changes:", error);
          alert(`เกิดข้อผิดพลาด: ${error.message}`);
          // Consider more sophisticated error handling/rollback depending on the error
      }
    },
    async deleteCourse(courseId, semesterKey) {
        // console.log('[Delete] Attempting to delete course ID:', courseId, 'from semester:', semesterKey);

        const courseToDelete = this.courses[semesterKey]?.find(c => c.id === courseId);
        const courseName = courseToDelete ? courseToDelete.name : `ID ${courseId}`;
        const semesterDisplay = this.formatSemesterDisplay(semesterKey);

        if (confirm(`คุณแน่ใจหรือไม่ว่าต้องการลบวิชา '${courseName}' ออกจาก ${semesterDisplay}?`)) {
            // console.log('[Delete] User confirmed deletion.');

            if (!this.courses[semesterKey]) {
                console.error(`[Delete Error] Semester array not found for key: ${semesterKey} during deletion.`);
                alert(`เกิดข้อผิดพลาด: ไม่พบข้อมูลภาคเรียน ${semesterKey}`);
                return;
            }

            try {
                // *** ENSURE THIS LINE USES BACKTICKS (`) ***
                const apiUrl = `${API_BASE_URL}/${semesterKey}/${courseId}`;
                console.log(`[Delete] Sending DELETE request to: ${apiUrl}`); // Log the CORRECT URL

                // *** ENSURE THIS LINE USES THE apiUrl VARIABLE ***
                const res = await fetch(apiUrl, {
                    method: 'DELETE',
                });

                console.log('[Delete] API Response Status:', res.status);
                console.log('[Delete] API Response OK:', res.ok);

                if (!res.ok) {
                    console.error(`[Delete Error] API responded with status ${res.status}`);
                    throw new Error(`HTTP error! status: ${res.status}`);
                }

                // --- Update local data AFTER successful API call ---
                const courseIndex = this.courses[semesterKey].findIndex(c => c.id === courseId);

                if (courseIndex !== -1) {
                    this.courses[semesterKey].splice(courseIndex, 1);
                    alert(`ลบวิชา '${courseName}' เรียบร้อยแล้ว`);
                    if (this.editingCourse && this.editingCourse.id === courseId) {
                        this.cancelEdit();
                    }
                } else {
                    console.warn(`[Delete Warning] Course ID ${courseId} not found locally after successful delete.`);
                    // await this.fetchCoursesForSemester(semesterKey);
                }
                // --- End of local data update ---

            } catch (error) {
                console.error("[Delete Catch Error] Error deleting course:", error);
                alert(`เกิดข้อผิดพลาดในการลบวิชา: ${error.message}`);
            }
        } else {
             // console.log('[Delete] User cancelled deletion.');
        }
    },
    async addCourse() {
      const { semester: semesterKey, id, name, credits, grade } = this.newCourse;

      if (!semesterKey || !id || !name || !credits || credits <= 0 || !grade) {
        alert("กรุณากรอกข้อมูลวิชาใหม่ให้ครบถ้วนและถูกต้อง (หน่วยกิตต้องเป็นเลขบวก และเลือกเกรด)");
        return;
      }
      if (!/^\d{8}$/.test(id)) {
          alert("รูปแบบรหัสวิชา (ID) ไม่ถูกต้อง (ต้องเป็นตัวเลข 8 หลัก)");
          return;
      }
      if (!this.courses[semesterKey]) {
         console.error(`Attempted to add course to non-existent semester key: ${semesterKey}`);
         this.courses[semesterKey] = []; // Initialize defensively
      }
      const existingCourse = this.courses[semesterKey].find(course => course.id === id);
      if (existingCourse) {
        alert(`รหัสวิชา (ID) '${id}' นี้มีอยู่แล้วใน ${this.formatSemesterDisplay(semesterKey)}`);
        return;
      }

      const courseDataToAdd = {
        id: id,
        name: name,
        credits: parseInt(credits),
        grade: grade
      };

      try {
          const res = await fetch(`${API_BASE_URL}/${semesterKey}`, {
              method: 'POST',
              headers: { 'Content-Type': 'application/json' },
              body: JSON.stringify(courseDataToAdd),
          });

          if (!res.ok) {
              if (res.status === 409) {
                 alert(`เกิดข้อผิดพลาด: รหัสวิชา (ID) '${id}' อาจมีอยู่แล้วในระบบสำหรับภาคเรียนนี้`);
              } else {
                 throw new Error(`HTTP error! status: ${res.status}`);
              }
              return;
          }

          // Update local data AFTER successful API call
          this.courses[semesterKey].push(courseDataToAdd);
          alert(`เพิ่มวิชา '${name}' ใน ${this.formatSemesterDisplay(semesterKey)} เรียบร้อย!`);

          // Reset Form
          this.newCourse.id = '';
          this.newCourse.name = '';
          this.newCourse.credits = null;
          this.newCourse.grade = '';
          this.$nextTick(() => {
            const idInput = document.getElementById('courseId');
            if(idInput) idInput.focus();
          });

      } catch (error) {
          console.error("Error adding course:", error);
          alert(`เกิดข้อผิดพลาดในการเพิ่มวิชา: ${error.message}`);
      }
    },
    async fetchCoursesForSemester(semesterKey) { // Optional helper
        try {
            const response = await fetch(`${API_BASE_URL}/${semesterKey}`);
            if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
            this.courses[semesterKey] = await response.json();
        } catch (error) {
            console.error(`Error fetching courses for ${semesterKey}:`, error);
            this.courses[semesterKey] = [];
        }
    },
    async loadInitialData() {
        this.isLoading.profile = true;
        this.isLoading.courses = true;
        try {
            // Fetch Profile
            const profileRes = await fetch(`${API_BASE_URL}/students`);
            if (!profileRes.ok) throw new Error(`Profile fetch failed: ${profileRes.status}`);
            this.profile = await profileRes.json();
            this.editedProfile = { ...this.profile };
            this.isLoading.profile = false;

            // Fetch Courses in parallel
            const coursePromises = SEMESTER_KEYS.map(key =>
                fetch(`${API_BASE_URL}/${key}`)
                    .then(res => res.ok ? res.json() : (console.warn(`Workspace failed for ${key}: ${res.status}`), []))
                    .catch(err => (console.error(`Workspace error for ${key}:`, err), []))
            );
            const coursesResults = await Promise.all(coursePromises);
            SEMESTER_KEYS.forEach((key, index) => {
                 this.courses[key] = coursesResults[index];
            });

             // Ensure default selections are valid
             if (!SEMESTER_KEYS.includes(this.selectedSemester)) this.selectedSemester = LATEST_SEMESTER_KEY;
             if (!SEMESTER_KEYS.includes(this.selectedEditSemester)) this.selectedEditSemester = LATEST_SEMESTER_KEY;
             if (!SEMESTER_KEYS.includes(this.newCourse.semester)) this.newCourse.semester = LATEST_SEMESTER_KEY;

        } catch (error) {
            console.error("Error loading initial data:", error);
            alert("เกิดข้อผิดพลาดในการโหลดข้อมูลเริ่มต้น กรุณาลองรีเฟรชหน้า");
            this.isLoading.profile = false; // Ensure loading state is false on error too
        } finally {
            this.isLoading.courses = false;
        }
    }
  },
  async mounted() {
     await this.loadInitialData();
  }
};
</script>

<style>
/* --- CSS Styles remain unchanged --- */
html {
  font-size: 17px;
}
body {
  line-height: 1.6;
  background: linear-gradient(-45deg, #d90fcf, #373ddc,#2e0cb7,#067334,#0ba23d, #e2df20, #e95d1d, #b70101);
  background-size: 400% 400%;
  animation: gradientAnimation 15s ease-in-out infinite;
}
@keyframes gradientAnimation {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
.main-wrapper {
    min-height: 100vh;
    background-color: transparent;
}
.container-fluid {
  width: 100%;
  max-width: 1400px;
  margin-left: auto;
  margin-right: auto;
  padding-left: 1rem;
  padding-right: 1rem;
  padding-bottom: 3rem;
}
.container-fluid.py-4 {
    padding-top: 1rem !important;
}
.content-section.card {
  width: 100%;
  margin-bottom: 2rem;
  border: none;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
  border-radius: 0.5rem;
  overflow: hidden;
  background-color: rgba(255, 255, 255, 0.98);
}
.card-header {
  padding: 1.1rem 1.35rem;
  background-color: #ffffff;
  border-bottom: 1px solid #dee2e6;
}
.card-header h2 {
  font-size: 1.65rem;
  font-weight: 600;
  margin-bottom: 0;
  color: #343a40;
  display: flex;
  align-items: center;
}
.card-header h2 i {
  font-size: 1.5rem;
  margin-right: 0.75rem;
}
.profile-view .card-header i { color: var(--bs-primary); }
.profile-edit .card-header i { color: var(--bs-primary); }
.courses-view .card-header i { color: var(--bs-info); }
.edit-courses-view .card-header i { color: var(--bs-success); }
.add-courses-view .card-header i { color: var(--bs-danger); }
.home-view .card-header i { color: var(--bs-primary); }
.card-body {
  padding: 1.5rem;
}
.card-body.p-0 {
    padding: 0 !important;
}
.table-card-body {
    padding-top: 1rem;
}
.home-view .row.align-items-stretch {
    display: flex;
}
.home-view .home-image-col {
    display: flex;
    padding: 0;
}
.home-view .welcome-image-fill {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
}
.home-view .home-text-col {
    background-color: #ffffff;
}
.profile-view.large-profile .card-body {
    padding: 2rem;
}
@media (min-width: 768px) {
    .profile-view.large-profile .card-body {
        padding: 2.5rem 3rem;
    }
}
.profile-pic-display-large {
    max-width: 250px;
    width: 100%;
    height: auto;
    border: 6px solid white;
    box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    margin-left: auto;
    margin-right: auto;
    display: block;
}
.profile-info-list .list-group-item {
    background-color: transparent;
    border-bottom: 1px dashed #e0e0e0;
    padding-left: 0;
    padding-right: 0;
    padding-top: 1.1rem;
    padding-bottom: 1.1rem;
}
.profile-info-list .list-group-item:last-child {
    border-bottom: none;
}
.profile-list-icon {
    font-size: 1.75rem;
    margin-top: 0.1rem;
    min-width: 35px;
    text-align: center;
}
.profile-info-list .profile-info-label {
    font-size: 0.9rem;
    color: #6c757d;
    font-weight: 500;
    margin-bottom: 0.15rem !important;
    min-width: auto;
}
.profile-info-list .profile-info-value {
    color: #343a40;
    font-weight: 500;
    word-break: break-word;
}
.btn-group {
  width: 100%;
  max-width: 900px;
  margin-left: auto;
  margin-right: auto;
  display: flex;
  flex-wrap: wrap;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  border-radius: 0.375rem;
  overflow: hidden;
}
.btn-group .btn {
  flex-grow: 1;
  text-align: center;
  border-radius: 0 !important;
  padding: 0.9rem 1rem;
  font-size: 1.05rem;
  font-weight: 500;
  transition: background-color 0.2s ease, color 0.2s ease, border-color 0.2s ease;
  border: none;
  border-right: 1px solid #dee2e6;
  margin-bottom: -1px;
  margin-right: -1px;
}
@media (min-width: 768px) {
    .btn-group .btn:last-child {
        border-right: none;
    }
}
.btn-group .btn:not(.active) {
  background-color: #ffffff;
  border: 1px solid transparent;
}
.btn-group .btn.btn-outline-primary:not(.active) { border-color: var(--bs-primary); color: var(--bs-primary);}
.btn-group .btn.btn-outline-success:not(.active) { border-color: var(--bs-success); color: var(--bs-success);}
.btn-group .btn.btn-outline-danger:not(.active) { border-color: var(--bs-danger); color: var(--bs-danger);}
.btn-group .btn:not(:last-child):not(.active) {
    border-right: 1px solid #dee2e6;
}
.btn-group .btn:not(.active):hover {
  background-color: #f1f1f1;
}
.btn-group .btn.active {
    font-weight: 600;
    color: #fff;
    border-color: transparent;
    border-right: 1px solid rgba(0, 0, 0, 0.1);
}
.form-label, .col-form-label {
  font-size: 0.95rem;
  margin-bottom: 0.5rem;
  font-weight: 500;
  color: #495057;
}
.form-control, .form-select {
  font-size: 1rem;
  padding: 0.6rem 0.9rem;
  min-height: calc(1.6em + 1.2rem + 2px);
  border: 1px solid #ced4da;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}
.form-control:focus, .form-select:focus {
    border-color: #86b7fe;
    box-shadow: 0 0 0 0.25rem rgba(13, 110, 253, 0.25);
}
.row.mb-3 {
  margin-bottom: 1.35rem !important;
}
.edit-course-form-container {
    box-shadow: 0 4px 15px rgba(var(--bs-warning-rgb), 0.15); /* Corrected variable name */
}
.table-responsive {
  margin-top: 1.5rem;
  margin-bottom: 1.5rem;
}
.table {
    border-collapse: separate;
    border-spacing: 0;
    border-radius: 0.375rem;
    overflow: hidden;
    border: 1px solid #dee2e6;
}
.table th,
.table td {
  vertical-align: middle;
  padding: 0.9rem 1.1rem;
  font-size: 1rem;
  border-top: 1px solid #dee2e6;
  border-bottom: none;
  border-left: none;
  border-right: none;
}
.table thead th {
  font-weight: 600;
  font-size: 1.05rem;
  background-color: #e9ecef;
  border-top: none;
  border-bottom: 2px solid #dee2e6;
}
.table tbody tr {
    transition: background-color 0.2s ease;
}
.table tbody tr:hover {
    background-color: rgba(var(--bs-primary-rgb), 0.06);
}
.gpa-box {
  width: fit-content;
  min-width: 250px;
  border: 1px solid #d1d9e1;
  background-color: #f1f5f9 !important;
  padding: 1rem 1.5rem !important;
  margin-top: 1.5rem;
  margin-left: auto;
  margin-right: auto;
  border-radius: 0.375rem;
  box-shadow: inset 0 1px 2px rgba(0,0,0,0.05);
}
.gpa-box p {
  font-size: 1.1rem;
  margin-bottom: 0.4rem;
  color: #212529;
}
.gpa-box p:last-child {
    margin-bottom: 0;
}
.btn-sm {
  padding: 0.4rem 0.8rem;
  font-size: 0.9rem;
  display: inline-flex;
  align-items: center;
}
.btn:not(.btn-sm) {
    padding: 0.7rem 1.2rem;
    font-size: 1.05rem;
    display: inline-flex;
    align-items: center;
    transition: filter 0.2s ease, transform 0.1s ease;
}
.btn:hover {
    filter: brightness(95%);
}
.btn:active {
    transform: scale(0.98);
    filter: brightness(90%);
}
form .d-flex.justify-content-end {
  margin-top: 1.75rem;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
.slide-fade-enter-active {
  transition: all 0.4s ease-out;
}
.slide-fade-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(20px);
  opacity: 0;
}
</style>