<template>
	<div
		v-if="loaded"
		class="module"
	>
		<div class="title">
			<h1>{{ module.Name }}</h1>
			<span>{{ module.UE.Label }}</span>
			<div class="time">
				<p>{{ module.__Nb_jour_5h_ou_7h }} jours</p>
				|
				<p>{{ module.__Heures_TD }} h</p>
			</div>
		</div>

		<div id="container">
			<div class="buttons">
				<button
					type="button"
					@click="changeMenuStatus('obj_gen')"
					:class="{ active: menu_status == 'obj_gen' }"
				>
					Objectif Général
				</button>
				<button
					type="button"
					@click="changeMenuStatus('obj_peda')"
					:class="{ active: menu_status == 'obj_peda' }"
				>
					Contenu
				</button>
				<button
					type="button"
					@click="changeMenuStatus('requis')"
					:class="{ active: menu_status == 'requis' }"
				>
					Pré-requis
				</button>
				<button
					type="button"
					@click="changeMenuStatus('biblio')"
					:class="{ active: menu_status == 'biblio' }"
				>
					Bibliographie
				</button>
				<button
					type="button"
					@click="changeMenuStatus('competences')"
					:class="{ active: menu_status == 'competences' }"
				>
					Compétences Visées
				</button>
				<button
					type="button"
					@click="changeMenuStatus('method')"
					:class="{ active: menu_status == 'method' }"
				>
					Méthodes Pédagogiques
				</button>
				<button
					type="button"
					@click="changeMenuStatus('seance')"
					:class="{ active: menu_status == 'seance' }"
				>
					Déroulement du module
				</button>
			</div>
			<form @submit.prevent="sendForm()">
				<!-- Obj Gén -->
				<div v-show="menu_status == 'obj_gen'">
					<label for="objectif_general">Objectif Général :</label>
					<textarea
						@input="emitSocket()"
						id="objectif_general"
						v-model="form.objectif_general"
						:disabled="module.Complete == 'Complete'"
					></textarea>
				</div>

				<!-- Obj Pédago -->
				<div v-show="menu_status == 'obj_peda'">
					<label for="objectifs_pedagogiques">Contenu :</label>
					<textarea
						@input="emitSocket()"
						id="objectifs_pedagogiques"
						v-model="form.objectifs_pedagogiques"
						:disabled="module.Complete == 'Complete'"
					></textarea>
				</div>

				<!-- Pré-requis -->
				<div v-show="menu_status == 'requis'">
					<label for="pre_requis">Pré-requis :</label>
					<textarea
						@input="emitSocket()"
						id="pre_requis"
						v-model="form.pre_requis"
						:disabled="module.Complete == 'Complete'"
					></textarea>
				</div>

				<!-- Biblio -->
				<div v-show="menu_status == 'biblio'">
					<label for="bibliographie">Bibliographie :</label>
					<textarea
						@input="emitSocket()"
						id="bibliographie"
						v-model="form.bibliographie"
						:disabled="module.Complete == 'Complete'"
					></textarea>
				</div>

				<!-- Compétences Visées -->
				<div v-show="menu_status == 'competences'">
					<label for="competences_visees">Compétences Visées :</label>
					<textarea
						@input="emitSocket()"
						id="competences_visees"
						v-model="form.competences_visees"
						:disabled="module.Complete == 'Complete'"
					></textarea>
				</div>

				<!-- Méthodes Pédagogiques -->
				<div v-show="menu_status == 'method'">
					<label for="methodes_pedagogiques">Méthodes Pédagogiques :</label>
					<div class="checkboxes">
						<label>
							<input
								type="checkbox"
								value="Enseignement en face à face"
								v-model="form.methodes_pedagogiques"
								:disabled="module.Complete == 'Complete'"
								@change="emitSocket()"
							/>
							Enseignement en face à face
						</label>
						<label>
							<input
								type="checkbox"
								value="Pédagogie par projets"
								v-model="form.methodes_pedagogiques"
								:disabled="module.Complete == 'Complete'"
								@change="emitSocket()"
							/>
							Pédagogie par projets
						</label>
						<label>
							<input
								type="checkbox"
								value="Pédagogie inversée"
								v-model="form.methodes_pedagogiques"
								:disabled="module.Complete == 'Complete'"
								@change="emitSocket()"
							/>
							Pédagogie inversée
						</label>
						<label>
							<input
								type="checkbox"
								value="Blended Learning"
								v-model="form.methodes_pedagogiques"
								:disabled="module.Complete == 'Complete'"
								@change="emitSocket()"
							/>
							Blended Learning
						</label>
						<label>
							<input
								type="checkbox"
								value="100% en ligne"
								v-model="form.methodes_pedagogiques"
								:disabled="module.Complete == 'Complete'"
								@change="emitSocket()"
							/>
							100% en ligne
						</label>
						<label>
							<input
								type="checkbox"
								value="Autre"
								v-model="form.methodes_pedagogiques"
								:disabled="module.Complete == 'Complete'"
								@change="emitSocket()"
							/>
							Autre
						</label>
					</div>
					<div v-if="form.methodes_pedagogiques?.includes('Autre')">
						<label for="autre">Autre :</label>
						<textarea
							@input="emitSocket()"
							id="autre"
							v-model="form.autre"
							:disabled="module.Complete == 'Complete'"
						></textarea>
					</div>
				</div>
				<div
					v-show="menu_status == 'seance'"
					class="seances"
				>
					<div
						v-for="seance in form.seances"
						:key="seance.id"
						class="seance"
						:disabled="module.Complete == 'Complete'"
						@input="emitSocket()"
					>
						<div class="num_seance">
							<label for="num_seance">Numéro de la séance :</label>
							<input
								@input="emitSocket()"
								type="number"
								id="num_seance"
								v-model="seance.NumeroSeance"
								:disabled="module.Complete == 'Complete'"
							/>
						</div>
						<div class="duration_seance">
							<label for="duration_seance">Durée de la séance :</label>
							<input
								@input="emitSocket()"
								type="time"
								id="duration_seance"
								v-model="seance.Durée"
								:disabled="module.Complete == 'Complete'"
							/>
						</div>
						<div class="personal_work">
							<label for="personal_work">Travail personnel :</label>
							<input
								@input="emitSocket()"
								type="text"
								id="personal_work"
								v-model="seance.TravailPersonnel"
								:disabled="module.Complete == 'Complete'"
							/>
						</div>
						<div class="themes">
							<label for="themes">Thèmes</label>
							<input
								@input="emitSocket()"
								id="themes"
								v-model="seance.Thèmes"
								:disabled="module.Complete == 'Complete'"
							/>
						</div>
						<div class="actions">
							<label for="actions">Actions: </label>
							<input
								@input="emitSocket()"
								id="actions"
								v-model="seance.Actions"
								:disabled="module.Complete == 'Complete'"
							/>
						</div>
						<!-- new seance button -->
						<!-- delete seance button -->
						<button
							type="button"
							@click="deleteSeance(seance.id), emitSocket()"
							class="delete_seance"
							:disabled="module.Complete == 'Complete'"
						>
							Retirer la séance -
						</button>
					</div>
					<button
						type="button"
						@click="addSeance(), emitSocket()"
						class="add_seance"
						:disabled="module.Complete == 'Complete'"
					>
						Nouvelle Seance +
					</button>
				</div>

				<!-- Send Button -->
				<div class="send_buttons">
					<input
						v-if="!submit || !module.Complete == 'Complete'"
						type="submit"
						value="Enregistrer"
						class="btn notsubmit"
						:disabled="module.Complete == 'Complete'"
					/>
					<input
						v-else
						value="Enregistré"
						class="btn submit"
						:disabled="module.Complete == 'Complete'"
					/>

					<input
						v-if="module.Complete != 'Complete'"
						type="button"
						@click="completeModule()"
						value="Soumettre le syllabus"
						class="btn notcomplete"
						:disabled="module.Complete == 'Complete'"
					/>
					<input
						v-else
						value="Soumis"
						class="btn completed"
					/>
				</div>
			</form>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'ModuleView',
		data() {
			return {
				menu_status: 'obj_gen',
				form: {
					methodes_pedagogiques: [],
					seances: [],
				},
				module: null,
				loaded: false,
				submit: false,
			}
		},
		methods: {
			sendForm(status) {
				this.$airtable(process.env.VUE_APP_AIRTABLE_MODULE_TABLE).update([
					{
						id: this.module.module_id,
						fields: {
							'Objectifs Pedagogiques': this.form.objectifs_pedagogiques,
							'Objectif General': this.form.objectif_general,
							'Pre-requis': this.form.pre_requis,
							Bibliographie: this.form.bibliographie,
							'Competences Visees': this.form.competences_visees,
							'Methodes Pedagogiques': this.form.methodes_pedagogiques,
							'Autre Methode Pedagogique':
								this.form.methodes_pedagogiques?.includes('Autre')
									? this.form.autre
									: null,
						},
					},
				])
				// Send seances
				if (this.module.seances) {
					this.module.seances.forEach((db_seance) => {
						if (
							!this.form.seances.some(
								(form_seance) => form_seance.id == db_seance.id
							)
						) {
							this.$airtable(
								process.env.VUE_APP_AIRTABLE_SEANCES_TABLE
							).destroy([db_seance.id])
							this.module.seances = this.module.seances.filter(
								(seance) => seance.id != db_seance.id
							)
						}
					})
				}
				this.form.seances.forEach((seance) => {
					if (seance.New) {
						this.$airtable(process.env.VUE_APP_AIRTABLE_SEANCES_TABLE).create([
							{
								fields: {
									NumeroSeance: seance.NumeroSeance,
									Durée: seance.Durée,
									TravailPersonnel: seance.TravailPersonnel,
									Thèmes: seance.Thèmes,
									Actions: seance.Actions,
									Module: [this.module.module_id],
								},
							},
						])
					} else {
						this.$airtable(process.env.VUE_APP_AIRTABLE_SEANCES_TABLE).update([
							{
								id: seance.id,
								fields: {
									NumeroSeance: seance.NumeroSeance,
									Durée: seance.Durée,
									TravailPersonnel: seance.TravailPersonnel,
									Thèmes: seance.Thèmes,
									Actions: seance.Actions,
								},
							},
						])
					}
				})
				this.submit = true
				if (status == 'completed') {
					this.$airtable(process.env.VUE_APP_AIRTABLE_MODULE_TABLE).update([
						{
							id: this.module.module_id,
							fields: {
								Complete: 'Complete',
							},
						},
					])
					this.module.Complete = 'Complete'
				}
			},
			emitSocket() {
				this.submit = false
				this.$socket.emit('obj', this.form)
			},
			changeMenuStatus(status) {
				this.menu_status = status
			},
			addSeance() {
				this.form.seances.push({
					id: Math.random().toString(36).substring(7),
					NumeroSeance: this.form.seances.length + 1,
					Durée: '',
					TravailPersonnel: '',
					Thèmes: '',
					Actions: '',
					New: true,
				})
			},
			deleteSeance(id) {
				console.log(id)
				this.form.seances = this.form.seances.filter(
					(seance) => seance.id != id
				)
			},
			completeModule() {
				confirm('Êtes-vous sûr de vouloir valider ce module ?')
					? this.sendForm('completed')
					: null
			},
		},
		async created() {
			// Get module
			this.$airtable(process.env.VUE_APP_AIRTABLE_MODULE_TABLE).find(
				this.$route.params.id,
				(err, record) => {
					if (err) {
						console.error(err)
						return
					}
					this.module = record.fields

					for (var key in this.module) {
						// replace all spaces and special chars by underscores and lowercase
						this.module[key.replace(/[^a-zA-Z0-9]/g, '_').toLowerCase()] =
							this.module[key]
					}

					// Put data in form
					Object.keys(this.module).forEach((key) => {
						this.form[key] = this.module[key]
					})

					// Get UE
					this.$airtable(process.env.VUE_APP_AIRTABLE_UE_TABLE).find(
						this.module.UE,
						(err, record) => {
							if (err) {
								console.error(err)
								return
							}
							this.module.UE = record.fields
						}
					)

					let seances = this.module.seances ?? []
					seances.forEach((seance) => {
						this.$airtable(process.env.VUE_APP_AIRTABLE_SEANCES_TABLE).find(
							seance,
							(err, record) => {
								if (err) {
									console.error(err)
									return
								}
								if (!record || !record.fields) return
								record.fields.id = record.id
								this.module.seances.push(record.fields)
								this.module.seances.splice(
									this.module.seances.indexOf(seance),
									1
								)
							}
						)
					})

					// Finish Loader
					setTimeout(() => {
						this.module.seances?.sort((a, b) => a.NumeroSeance - b.NumeroSeance)
						this.loaded = true
					}, 2000)

					this.$socket.on('data', (data) => {
						Object.keys(data).forEach((key) => {
							this.form[key] = data[key]
						})
					})
				}
			)
		},
	}
</script>

<style>
	.title {
		margin-top: 50px;
		font-size: 1.2rem;
		font-weight: 600;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 10px;
		margin-bottom: 20px;
	}

	#container {
		display: flex;
		align-items: flex-start;
		justify-content: space-evenly;
		margin-top: 80px;
	}

	div {
		width: 100%;
	}
	span {
		background-color: aliceblue;
		padding: 0.5rem;
		border-radius: 5px;
		color: #505050;
	}

	textarea {
		border-radius: 5px;
		padding: 20px;
		box-shadow: 4px 8px 8px;
		width: 100%;
		resize: none;
		height: 20vh;
		margin-bottom: 50px;
	}

	form {
		margin: 20px auto;
		display: flex;
		flex-direction: column;
		align-items: center;
		width: 60%;
	}

	.buttons {
		position: sticky;
		top: 50px;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 20px;
		justify-content: center;
		width: 30%;
	}

	.buttons button {
		width: 80%;
		margin: 0 auto;
		padding: 20px;
		border-radius: 5px;
		background-color: #f2f2f2;
		border: none;
	}

	.active {
		background-color: #485a71 !important;
		color: #f2f2f2;

	}

	.buttons button:hover {
		background-color: #485a71;
		color: #f2f2f2;
	}

	form label {
		font-size: 1rem;
		font-weight: 600;
	}

	.checkboxes {
		padding: 50px 0;
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		gap: 20px;
		justify-items: start;
	}

	.checkboxes label {
		font-size: 1rem;
		font-weight: 400;
	}

	.checkboxes input {
		margin-right: 10px;
		height: 20px;
		width: 20px;
		border-radius: 5px;
	}

	.send_buttons {
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 20px;
	}

	.btn {
		width: 20vw;
		margin-top: 20px;
		padding: 10px;
		border-radius: 5px;
		border: none;
		text-align: center;
	}

	.submit {
		background-color: #4caf50;
		color: white;
	}

	.notsubmit {
		background-color: #2196f3;
		color: white;
	}

	.completed {
		background-color: #505050;
		color: white;
	}

	.notcomplete {
		background-color: #ed1d1d;
		color: white;
	}

	.time {
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 30px;
	}

	.skills {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: space-around;
		border-radius: 5px;
		padding: 20px;
		box-shadow: 4px 8px 8px;
		width: 100%;
		height: 20vh;
		margin-bottom: 50px;
	}

	.skill {
		display: flex;
		gap: 20px;
		align-items: center;
	}

	.skill span {
		cursor: pointer;
	}

	.skill p {
		margin: 0;
	}

	.add-skill {
		margin: 50px 0 100px 0;
	}

	.seance {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: space-around;
		border-radius: 5px;
		padding: 20px;
		width: 100%;
	}

	.add_seance, .delete_seance {
		padding: 10px 20px;
		border-radius: 5px;
		border: 1px solid #505050;
		margin-bottom: 50px;
	}

	.add_seance:hover, .delete_seance:hover {
		background-color: #505050;
		color: white;
	}

	.num_seance, .duration_seance, .personal_work, .themes, .actions {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-bottom: 10px;
	}




</style>
