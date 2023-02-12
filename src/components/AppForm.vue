<template>
	<v-form v-model="valid" ref="form" @submit.prevent="onSubmit">
		<v-container class="mt-10">
			<v-row justify="space-around">
				<v-col cols="5">
					<v-row justify="center">
						<v-text-field
							v-model="formData.lastname"
							label="Фамилия"
							:rules="nameRules"
							required
						></v-text-field>
					</v-row>

					<v-row justify="center">
						<v-text-field
							v-model="formData.firstname"
							label="Имя"
							:rules="nameRules"
							required
						></v-text-field>
					</v-row>

					<v-row justify="center">
						<v-text-field
							v-model="formData.middlename"
							:rules="middlenameRules"
							label="Отчество"
						></v-text-field>
					</v-row>

					<v-row justify="center">
						<v-text-field
							v-model="formData.email"
							label="E-mail"
							:rules="emailRules"
							required
						></v-text-field>
					</v-row>

					<v-row>
						<v-text-field
							v-model="formData.date"
							type="date"
							label="Дата рождения"
							:rules="dateRules"
							required
						></v-text-field>
					</v-row>
				</v-col>

				<v-col cols="5">
					<v-row>
						<v-select
							v-model="formData.select"
							:hint="`${label}`"
							:items="items"
							item-title="group"
							item-value="group"
							label="Группа клиентов"
							persistent-hint
							return-object
							single-line
							class="mb-10"
						></v-select>
					</v-row>

					<v-row>
						<v-radio-group
							v-model="formData.gender"
							column
							class="mb-5"
						>
							<template v-slot:label>
								<div>Выберите пол</div>
							</template>
							<v-radio
								label="М"
								color="blue"
								value="male"
							></v-radio>
							<v-radio
								label="Ж"
								color="blue"
								value="female"
							></v-radio>
						</v-radio-group>
					</v-row>

					<v-row>
						<v-checkbox
							v-model="formData.sendSms"
							label="Не отправлять СМС"
							color="blue"
							value="true"
							hide-details
						></v-checkbox>
					</v-row>
				</v-col>
			</v-row>
			<v-row justify="center">
				<div class="d-flex flex-column">
					<v-btn
						color="success"
						class="mt-4"
						block
						type="submit"
						:disabled="!valid"
					>
						{{ regBtnText }}
					</v-btn>

					<v-btn color="warning" class="mt-4" block @click="reset">
						Сбросить форму
					</v-btn>
				</div>
			</v-row>
		</v-container>
	</v-form>
</template>

<script>
export default {
	data: () => ({
		valid: false,
		formData: {
			firstname: '',
			lastname: '',
			middlename: '',
			email: '',
			gender: 'male',
			date: '',
			sendSms: false,
			select: { group: 'VIP' },
		},
		label: 'Группа клиентов',
		items: [{ group: 'VIP' }, { group: 'Проблемные' }, { group: 'ОМС' }],
		nameRules: [
			(v) => !!v || 'Имя и фамилия обязательны!',
			(v) =>
				(v && v.length <= 100) ||
				'Имя или фамилия должно быть не длинее 100 символов!',
		],
		middlenameRules: [
			(v) =>
				(v && v.length <= 100) ||
				v.length === 0 ||
				'Отчество должно быть не длинее 100 символов!',
		],
		emailRules: [
			(v) => !!v || 'Электронная почта обязательна!',
			(v) =>
				(v && /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v)) ||
				'Неверный адрес почты!',
		],
		dateRules: [
			(v) => !!v || 'Дата рождения ообязательна!',
			(v) =>
				(v && new Date(v).getFullYear() < new Date().getFullYear()) ||
				'Выбранный год должен быть меньше текущего!',
		],
		regBtnText: 'Зарегистрировать',
	}),

	methods: {
		reset() {
			this.$refs.form.reset();
		},
		onSubmit() {
			this.regBtnText = 'Отправка данных...';
			this.axios
				.post(
					'https://3e9b7e01-7986-4061-bb1a-4a607d620ef3.mock.pstmn.io',
					this.formData
				)
				.then(() => {
					this.regBtnText = 'Данные успешно отправлены!';
				})
				.catch((err) => console.log(err));
		},
		selectGroup(selectedGroup) {
			this.formData.select.push(selectedGroup);
		},
	},
};
</script>
