<template>
	<form id="form" @submit.prevent="submitForm()">

		<header class="row-header">Пользователь</header>

		<div class="row">
			<InputText label="Фамилия" :vld="$v.lastname" :val="(lastname)=>{this.lastname = lastname}" />
			<InputText label="Имя" :vld="$v.firstname" :val="(firstname)=>{this.firstname = firstname}" />
			<InputText label="Отчество" :vld="$v.middlename" :val="(middlename)=>{this.middlename = middlename}" />
		</div>

		<div class="row">
			<InputDate label="Дата рождения"
				:vld="$v.birthDate"
				:val="(birthDate)=>{this.birthDate = birthDate}" width="10rem" />
			<InputText label="Номер телефона"
				:vld="$v.phone" pref="+7"
				:val="(phone)=>{this.phone = phone}" width="8rem" />
			<SelectText label="Лечащий врач"
				:vld="$v.doctor"
				:val="(doctor)=>{this.doctor = doctor}"
				:values="doctors" />
		</div>

		<div class="row">
			<SelectText label="Группа клиентов"
				:vld="$v.client"
				:val="(client)=>{this.client = client}"
				:values="clients" :mult="true" />
			<div class="row-item">
				<label class="row-item__label">Пол</label>
				<input class="row-item__radio" id="male" type="radio" value="male" v-model="gender">
				<label for="male">Мужской</label>
				<br>
				<input class="row-item__radio" id="fem" type="radio" value="female" v-model="gender">
				<label for="fem">Женский</label>
			</div>
			<div class="row-item">
				<input class="checkbox" type="checkbox" v-model="sendSMS">
				<label for="checkbox">Не отправлять СМС</label>
			</div>
		</div>

		<header class="row-header">Адрес</header>

		<div class="row">
			<InputText label="Индекс" :vld="$v.stateIndex" :val="(stateIndex)=>{this.stateIndex = stateIndex}" width="5rem" />
			<InputText label="Страна" :vld="$v.country" :val="(country)=>{this.country = country}" />
			<InputText label="Область" :vld="$v.stateName" :val="(stateName)=>{this.stateName = stateName}" />
			<InputText label="Город" :vld="$v.city" :val="(city)=>{this.city = city}" />
			<InputText label="Улица" :vld="$v.street" :val="(street)=>{this.street = street}" width="15rem"/>
			<InputText label="Дом" :vld="$v.building" :val="(building)=>{this.building = building}" width="5rem"/>
		</div>

		<header class="row-header">Паспорт</header>

		<div class="row">
			<SelectText label="Тип документа"
				:vld="$v.docType"
				:val="(docType)=>{this.docType = docType}"
				:values="docTypes" />
			<InputText label="Серия" :vld="$v.docSer" :val="(docSer)=>{this.docSer = docSer}" width="4rem" />
			<InputText label="Номер" :vld="$v.docNumber" :val="(docNumber)=>{this.docNumber = docNumber}" width="6rem" />
			<InputDate label="Дата выдачи" :vld="$v.docDate" :val="(docDate)=>{this.docDate = docDate}" width="10rem" />
			<InputText label="Кем выдан" :vld="$v.docCreator" allow="customSpaceAlpha"
				:val="(docCreator)=>{this.docCreator = docCreator}" width="15rem" />
		</div>

		<div class="row">
			<p class="submit-message submit-message__error" v-if="submitStatus === 'ERROR'">Пожалуйста заполните обязательные поля.</p>
			<p class="submit-message" v-if="submitStatus === 'PENDING'">Отправляем...</p>
		</div>

		<div class="row">
			<button class="submit-button" type="submit" :disabled="submitStatus === 'PENDING'">
				Создать пользователя
			</button>
			<input class="clear-button" type="reset" @click="clearForm">
		</div>
	</form>
</template>

<script>
import { required, minLength, maxLength, numeric } from 'vuelidate/lib/validators'
import InputText from './InputText'
import InputDate from './InputDate'
import SelectText from './SelectText'

export default {
	name: 'CreateUser',
	components: {
		InputText,
		InputDate,
		SelectText
	},
  	data() {
    	return {
			lastname: '',
			firstname: '',
			middlename: '',
			birthDate: '',
			phone: '',
			gender: '',
			client: [],
			clients: ['VIP', 'Проблемные', 'ОМС'],
			doctor: '',
			doctors: ['Иванов', 'Захаров', 'Чернышева'],
			sendSMS: false,
			stateIndex: '',
			country: '',
			stateName: '',
			city: '',
			street: '',
			building: '',
			docType: '',
			docTypes: ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение'],
			docSer: '',
			docNumber: '',
			docCreator: '',
			docDate: '',
			submitStatus: null
    	}
	},
  	validations: {
		lastname: {
			required,
			minLength: minLength(2),
			customAlpha: val => /^[а-яёa-z]*$/i.test(val),
		},
		firstname: {
			required,
			minLength: minLength(2),
			customAlpha: val => /^[а-яёa-z]*$/i.test(val),
		},
		middlename: {
      		minLength: minLength(2),
			customAlpha: val => /^[а-яёa-z]*$/i.test(val)
		},
		birthDate: {
      		required
    	},
    	phone: {
      		required,
			numeric,
      		minLength: minLength(10),
      		maxLength: maxLength(10),
		},
		doctor: {},
		client: {
			required,
		},
    	stateIndex: {
			numeric,
			minLength: minLength(6),
			maxLength: maxLength(6),
    	},
    	country: {
			minLength: minLength(2),
			customAlpha: val => /^[а-яёa-z]*$/i.test(val)
    	},
    	stateName: {
			minLength: minLength(2),
			customAlpha: val => /^[а-яёa-z]*$/i.test(val)
    	},
    	city: {
			required,
			minLength: minLength(2),
			customAlpha: val => /^[а-яёa-z]*$/i.test(val)
    	},
    	street: {
			minLength: minLength(2),
			customAlpha: val => /^[а-яёa-z]*$/i.test(val)
    	},
    	building: {
      		numeric
		},
		docType: {
      		required
    	},
    	docSer: {
			numeric,
			minLength: minLength(4),
      		maxLength: maxLength(4)
		},
    	docNumber: {
      		numeric,
			minLength: minLength(6),
      		maxLength: maxLength(6)
		},
    	docCreator: {
			minLength: minLength(2),
			customSpaceAlpha: val => /^[а-яёa-z\s]*$/i.test(val)
		},
		docDate: {
			required
    	},
  	},
  	methods: {
		submitForm() {
      		this.$v.$touch()
      		if (this.$v.$invalid) {
        		this.submitStatus = 'ERROR'
      		} else {
        		this.submitStatus = 'PENDING'
				let user = {}
				user.lastname = this.lastname
				user.firstname = this.firstname
				user.middlename = this.middlename
				user.birthDate = this.birthDate
				user.phone = this.phone
				user.gender = this.gender
				user.client = this.client
				user.doctor = this.doctor
				user.sendSMS = this.sendSMS
				user.address = {}
				user.address.stateIndex = this.stateIndex
				user.address.country = this.country
				user.address.stateName = this.stateName
				user.address.city = this.city
				user.address.street = this.street
				user.address.building = this.building
				user.document = {}
				user.document.docType = this.docType
				user.document.docSer = this.docSer
				user.document.docNumber = this.docNumber
				user.document.docCreator = this.docCreator
				user.document.docDate = this.docDate
				console.log(user)
        		setTimeout(() => {
          			this.submitStatus = 'OK'
					alert(`Создан пользователь: ${user.firstname} ${user.lastname}`)
					this.$nextTick(() => { document.getElementById('form').reset()})
				}, 500)
				this.$v.$reset()

      		}
		},
		clearForm() {
			this.lastname = ''
			this.firstname = ''
			this.middlename = ''
			this.birthDate = ''
			this.phone = ''
			this.gender = ''
			this.client = []
			this.doctor = ''
			this.sendSMS = false
			this.stateIndex = ''
			this.country = ''
			this.stateName = ''
			this.city = ''
			this.street = ''
			this.building = ''
			this.docType = ''
			this.docSer = ''
			this.docNumber = ''
			this.docCreator = ''
			this.docDate = ''
			this.submitStatus = null
			this.$v.$reset()
		}
  	}
}
</script>

<style lang="sass" >

$document-width: 50rem
$green: #56ab2f
$bg-color: #f8f8f8
$text-color: #555555
$error-color: #f79483

body
	margin: 0

.row-header
	background-color: $green
	color: white
	max-width: $document-width
	margin: 0 auto
	padding: 0.5rem
	font-size: 1.2rem
	text-align: left

.row
	display: flex
	flex-wrap: wrap
	justify-content: left
	max-width: $document-width
	margin: 0 auto
	padding: 0 1rem 0.5rem 0
	font-size: 1rem
	background-color: $bg-color
	justify-content: space-around
	color: $text-color

.row-item
  padding: 0.5rem

  &__label
    display: block
    text-align: left
    font-size: 1rem
    padding: 0.3rem 0.1rem

  &__error
    color: red
    font-size: 0.7rem
    color: $error-color
    min-width: max-content

  &__group
    &_error
      color: $error-color

  &__input
    border: 0.05rem solid
    border-radius: 0.2rem
    transition: border .1s ease
    box-sizing: border-box
    padding-left: 0.3rem
    color: $text-color

    &_error
      border-color: $error-color

    &:focus
      outline: none

input
	font-size: 1rem

select
	font-size: 1rem

button
	font-size: 1.2rem
	color: $text-color

.clear-button
	font-size: 1.2rem
	color: $text-color

.submit-message
	margin: 0 auto

	&__error
      color: $error-color

</style>
