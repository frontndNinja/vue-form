<template>
    <form @submit.prevent="handleSubmit">
        <label>Email: </label>
        <input type="email"
            v-model="email"
            required>
        <label>Password: </label>
        <input type="password"
            v-model="password"
            required>
        <div v-show="passwordErr">
            <p class="errMsg">{{passwordErr}}</p>
        </div>
        <label>Role: </label>
        <select v-model="role">
            <option value="Web designer">Web designer</option>
            <option value="Web developer">Web developer</option>
        </select>
        <label>Skills</label>
        <input type="text"
            v-model="tempSkill"
            @keyup="addSkill">
        <div class="pill-wrapper">
            <div v-for="(skill, index) in skills"
                :key="index"
                class="pill">
                <p @click="deleteSkill(skill)">{{skill}} 𝗫</p>
            </div>
        </div>
        <div v-show="skillsErr">
            <p class="errMsg">{{skillsErr}}</p>
        </div>
        <div class="terms">
            <input type="checkbox"
                v-model="terms">
            <label>Accept terms and conditions</label>
            <div v-show="termsErr">
                <p class="errMsg">{{termsErr}}</p>
            </div>
        </div>
        <label>Get info on</label>
        <div class="flex">
            <div>
                <input type="checkbox"
                    value="Tech"
                    v-model="subjects">
                <label>Tech</label>
            </div>
            <div>
                <input type="checkbox"
                    v-model="subjects"
                    value="Design">
                <label>Design</label>
            </div>
            <div>
                <input type="checkbox"
                    v-model="subjects"
                    value="Development">
                <label>Development</label>
            </div>
        </div>
        <div v-show="subjectsErr">
            <p class="errMsg">{{subjectsErr}}</p>
        </div>
        <div class="submit">
            <button>Create an account</button>
        </div>

    </form>
    <p>Email: {{email}}</p>
    <p>Password: {{password}}</p>
    <p>Role: {{role}}</p>
    <p>Terms: {{terms}}</p>
    <p>Subjects chosen: {{subjects}}</p>
</template>

<script>
export default {
    data() {
        return {
            email: '',
            password: '',
            role: 'Web developer',
            terms: false,
            subjects: [],
            tempSkill: '',
            skills: [],
            passwordErr: '',
            termsErr: '',
            skillsErr: '',
            subjectsErr: ''
        }
    },
    methods: {
        addSkill(e) {
            if (e.key === 'Enter' && this.tempSkill) {
                console.log(1)
                if (!this.skills.includes(this.tempSkill)) {
                    this.skills.push(this.tempSkill);
                } else {
                    alert('This skill was already added. Add a different one.')
                }
                this.tempSkill = ''
            }
        },
        deleteSkill(item) {
            console.log(item)
            this.skills = this.skills.filter(skill => skill !== item);
        },
        handleSubmit() {
            this.passwordErr = this.password.length > 12 ? '' : 'Password is too short'
            this.termsErr = this.terms ? '' : 'You need to check off the terms and conditions'
            this.skillsErr = this.skills.length > 0 ? '' : 'You need to choose at least one skill'
            this.subjectsErr = this.subjects.length > 0 ? '' : 'You need to choose at least one subject'

            if (this.passwordErr || this.skillsErr || this.subjectsErr || this.termsErr) {
                return;
            }
            const values = {
                'email': this.email,
                'password': this.password,
                'role': this.role,
                'skills': this.skills,
                'terms and conditions': this.terms,
                'subjects': this.subjects,
            }
            console.log(values)

        }
    }
}
</script>

<style scoped>
form {
    max-width: 420px;
    margin: 30px auto;
    background: white;
    text-align: left;
    padding: 40px;
    border-radius: 10px;
}

label {
    color: #aaa;
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.6rem;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
}

input,
select {
    display: block;
    padding: 10px 6px;
    width: 100%;
    border: none;
    border-bottom: 1px solid #ddd;
    color: #555;
    box-sizing: border-box;
}

select {
    padding: 10px 0px;
}

input[type="checkbox"] {
    display: inline-block;
    width: 16px;
    margin: 0 10px 0 0;
    position: relative;
    top: 2px;
}

.flex {
    display: flex;
    column-gap: 1rem;
    margin-bottom: 2rem;
}

.flex label {
    margin: 0;
    text-transform: initial;
    letter-spacing: 0px;
    font-weight: 400;
    font-size: .8rem;
    color: black
}

.pill-wrapper {
    flex-wrap: wrap;
    display: flex;
    align-items: center;
    padding-top: 5px;
}

.pill {
    border-radius: 30px;
    background: rgb(234, 234, 234);
    color: black;
    width: fit-content;
    padding: 5px 10px;
    margin: 5px 10px 5px 0;
}


.pill p {
    margin: 0;
    text-transform: uppercase;
    font-size: 12px;
    font-weight: 600;
    letter-spacing: 1;
}

button {
    background: rgb(15, 90, 251);
    color: white;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    border-radius: 20px;
}

.submit {
    text-align: center;
}

.errMsg {
    color: rgb(193, 20, 20);
    font-size: 12px;
}
</style>