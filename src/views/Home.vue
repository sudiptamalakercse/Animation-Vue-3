<template>
	<div class="container">
		<div class="row">
			<div class="col-md-12">
				<div class="mt-3"></div>
				<form @submit.prevent="onHobbySubmit()">
					<div class="form-group">
						<label>Enter Hobby</label>
						<input
							type="text"
							class="form-control"
							v-model="enteredHobbyValue"
						/>
					</div>
					<transition name="animetionBtnAdd" mode="out-in">
						<button class="btn btn-primary" v-if="animetionBtnAdd == true">
							Add Hobby
						</button>
						<button class="btn btn-primary" v-else>
							Add Hobby
						</button>
					</transition>
				</form>
				<div class="mt-3">
					<transition>
						<div
							v-if="hobbies.length === 0 && resultHobbies == 0"
							class="alert alert-warning"
						>
							No Hobbies Present
						</div>
					</transition>
					<h4 v-if="hobbies.length > 0">Hobbies List</h4>
					<transition>
						<div v-if="hobbies.length > 0">
							<transition-group
								name="list"
								tag="ul"
								v-if="hobbies.length > 0"
								class="list-group"
							>
								<li
									class="list-group-item"
									v-for="(hobby, index) in hobbies"
									:key="hobby"
									@click.prevent="removeHobby(index)"
								>
									<div>{{ hobby }} - {{ index }}</div>
									<input type="text" :ref="'v' + index" @click.stop />
									&nbsp;
									<span>
										<button
											class="btn btn-danger"
											@click.stop="removeHobby(index)"
										>
											X {{ hobby }} X
										</button>
									</span>
								</li>
							</transition-group>
						</div>
					</transition>
				</div>
				<transition name="animetionBtnSubmmit">
					<button
						class="btn btn-primary mt-3 mb-3"
						v-if="showSubmmit == true && animetionBtnSubmmit == true"
						@click="submmitDone"
					>
						Submmit
					</button>
				</transition>
				<transition>
					<div v-if="showResult == true && animetion == true">
						<h4>Submitted Hobbies List</h4>
						<ul class="list-group">
							<li
								class="list-group-item"
								v-for="hobby in resultHobbies"
								:key="hobby"
							>
								{{ hobby }}
							</li>
						</ul>
					</div>
				</transition>
				<div class="mt-3"></div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			hobbies: [],
			resultHobbies: [],
			enteredHobbyValue: '',
			showSubmmit: false,
			showResult: false,
			animetion: true,
			animetionBtnAdd: true,
			animetionBtnSubmmit: true
		};
	},
	methods: {
		onHobbySubmit() {
			this.animetionBtnAdd = !this.animetionBtnAdd;
			if (this.enteredHobbyValue) {
				this.hobbies.push(this.enteredHobbyValue);
				this.enteredHobbyValue = '';
				this.showResult = false;
				this.resultHobbies = [];
			} else {
				alert('Fill Up hobby fild');
			}
		},
		removeHobby(index) {
			this.hobbies.splice(index, 1);
			this.showResult = false;
			this.animetion = false;
			this.animetionBtnSubmmit = false;
		},
		submmitDone() {
			this.showResult = true;
			this.hobbies.forEach((hobby, key) => {
				let newkey = 'v' + key;
				let conbineValue;
				if (this.$refs[newkey].value) {
					let txtValue = this.$refs[newkey].value;
					conbineValue = hobby + ' - ' + txtValue;
				} else {
					conbineValue = hobby;
				}

				this.resultHobbies.push(conbineValue);
			});
			this.hobbies = [];
			this.animetionBtnSubmmit = false;
		}
	},
	watch: {
		hobbies: {
			handler(hobbies) {
				if (hobbies.length > 0) {
					this.showSubmmit = true;
				} else {
					this.showSubmmit = false;
				}
			},
			deep: true
		}
	},
	updated() {
		this.animetion = true;
		this.animetionBtnSubmmit = true;
	}
};
</script>
<style scoped>
.list-enter-from {
	opacity: 0;
	transform: translateY(50px);
}
.list-enter-to {
	opacity: 1;
	transform: translateY(0px);
}
.list-enter-active {
	transition: all 1s ease-in;
}
.list-leave-from {
	opacity: 1;
	transform: translateY(0px);
}
.list-leave-to {
	opacity: 0;
	transform: translateY(-50px);
}
.list-leave-active {
	transition: all 1s ease-out;
	position: absolute;
}
.list-move {
	transition: all 1s ease;
}
.v-enter-from {
	opacity: 0;
	transform: translateY(50px);
}
.v-enter-to {
	opacity: 1;
	transform: translateY(0px);
}
.v-enter-active {
	transition: all 1s ease-out;
}
.v-leave-from {
	opacity: 1;
	transform: translateY(0px);
}
.v-leave-to {
	opacity: 0;
	transform: translateY(-50px);
}
.v-leave-active {
	transition: all 1s ease-in;
}
.animetionBtnAdd-enter-from {
	transform: scaleX(1.2);
	opacity: 0;
}
.animetionBtnAdd-enter-to {
	transform: scaleX(1);
	opacity: 1;
}
.animetionBtnAdd-enter-active {
	transition: all 1.3s ease;
}
.animetionBtnAdd-leave-from {
	opacity: 1;
	transform: scaleX(1.2);
}
.animetionBtnAdd-leave-to {
	opacity: 0;
	transform: scaleX(1);
}
.animetionBtnAdd-leave-active {
	transition: all 1.3s ease;
}
.animetionBtnSubmmit-enter-from {
	opacity: 0;
}
.animetionBtnSubmmit-enter-to {
	opacity: 1;
}
.animetionBtnSubmmit-enter-active {
	transition: all 1.5s ease;
}
.animetionBtnSubmmit-leave-from {
	opacity: 1;
	transform: scaleX(1.2);
}
.animetionBtnSubmmit-leave-to {
	opacity: 0;
	transform: scaleX(1);
}
.animetionBtnSubmmit-leave-active {
	transition: all 1.5s ease;
}
</style>
