<template>
	<div class="container">
		<h3 class="text-center pt-3 pb-2 fixed-top">Realtime Chat App - Dimas</h3>
		<div class="chat-container">
			<div class="row justify-content-center">
				<div class="col-12 col-md-8 col-lg-5">
					<div class="chat-box mt-3 rounded" v-for="msg of msgContainer" :key="msg">
						<p>{{ msg }}</p>
					</div>
				</div>
			</div>
		</div>
		<form class="fixed-bottom mx-auto pb-3" @submit="postMessage">
			<input
				type="text"
				class="form-control mx-auto shadow-none mt-5 w-75"
				placeholder="Message..."
				v-model="msgValue"
			/>
		</form>
	</div>
</template>

<script>
import Gun from "gun";
const gun = Gun({
	peers: ["http://localhost:8000/gun"]
});

const db = gun.get("messages");

export default {
	name: "App",
	data: () => ({
		msgContainer: [],
		msgValue: ""
	}),
	beforeMount() {
		db.on((ms) => console.log(ms));
		db.map().on((ms) => this.msgContainer.push(ms));
	},
	methods: {
		postMessage(event) {
			event.preventDefault();

			db.set(this.msgValue);
			this.msgValue = "";
			window.scrollTo(-10, document.body.scrollHeight);
		}
	}
};
</script>

<style lang="scss">
.container {
	h3,
	form {
		background-color: #202124;
	}
	.chat-container {
		margin-top: 70px;
		margin-bottom: 102px;
	}
	.chat-box {
		p {
			text-align: center;
			font-size: 20px;
		}
		border: 2px solid white;
		padding: 15px;
	}
	.form-control {
		border-top: #fff !important;
		color: #333 !important;
		outline: none;
		border: #333;
	}
}
</style>
