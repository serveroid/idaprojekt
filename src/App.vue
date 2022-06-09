<template>
	<div class="container">
		<div style="display: flex;">
			<h1 class="add-form-title">Добавление товара</h1>
			
			<div @click="filter = !filter" style="position: absolute; right: 15px; z-index: 999;">
				<p>{{sortType}}</p>
				<div v-if="filter">
					<div><button @click="sortProduct('По цене min')">По цене min</button></div>
					<div><button @click="sortProduct('По цене max')">По цене max</button></div>
					<div><button @click="sortProduct('По наименованию')">По наименованию</button></div>
				</div>
			</div>
		</div>
		<div class="product-container">
			<div class="form">
				<form @submit.prevent class="add-form">
					<div class="add-form-block">
						<div class="form-text-block">
							<p class="form-text">Наименование товара</p>
							<div class="form-text-circle"></div>
						</div>
						<input v-bind:value="name" @input="name = $event.target.value" class="form-input" type="" name="" placeholder=" Введите наименование товара">
						<p v-if="!name.length" class="err-text">Поле является обязательным</p>
					</div>

					<div class="add-form-block">
						<div class="form-text-block">
							<p class="form-text">Описание товара</p>
							<div class="form-text-circle"></div>
						</div>
						<textarea v-bind:value="dis" @input="dis = $event.target.value" class="form-input dis-input" type="" name="" placeholder=" Введите описание товара"></textarea>
					</div>

					<div class="add-form-block">
						<div class="form-text-block">
							<div class="form-text-block">
								<p class="form-text">Ссылка на изображение товара</p>
								<div class="form-text-circle"></div>
							</div>
						</div>
						<input v-bind:value="uri" @input="uri = $event.target.value" class="form-input" type="" name="" placeholder=" Введите ссылку">
						<p v-if="!uri.length" class="err-text">Поле является обязательным</p>
					</div>

					<div class="add-form-block">
						<div class="form-text-block">
							<p class="form-text">Цена товара</p>
							<div class="form-text-circle"></div>
						</div>
						<input v-bind:value="price" @input="price = $event.target.value" class="form-input" type="number" name="" placeholder=" Введите цену">
						<p v-if="!price.length" class="err-text">Поле является обязательным</p>
					</div>

					<div class="add-form-block">
						<div v-if="name && uri && price">
							<button @click="addProduct" :disabled="false" class="button-add">
								<p class="button-add-text">Добавить товар</p>
							</button>
						</div>
						<div v-else>
							<button @click="addProduct" :disabled="true" class="button-add" style="background: #EEEEEE;">
								<p class="button-add-text" style="color: #B4B4B4;">Добавить товар</p>
							</button>
						</div>
					</div>
				</form>
			</div>

			<div class="posts">
				<div class="post" v-for="post in posts" :key="post.id">
					<img :src="post.uri" class="post-img">
					<p class="post-name">{{post.name}}</p>
					<p class="post-dis">{{post.dis}}</p>
					<p class="post-price">{{post.price}}<span> руб.</span></p>

					<button @click="delProduct(post)" style="position: absolute; right: 0; top: 0;">X</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				posts: [
					{id: Date.now(), name: 'ауауауа', dis: '23ч23с423с42с', uri: 'https://sun9-east.userapi.com/sun9-26/s/v1/if2/mkolWB8k7N0BqqDV2maSRrxJjRqYCRg4_BcqyB61XoFwSRkMjl6Lwmvi5UdHWE-1mC-MoCW3LAUJMqcELHwDiP7P.jpg?size=1280x853&quality=95&type=album', price: '300'},
					{id: Date.now()+1, name: 'укаукауауа', dis: '32с423с42с', uri: 'https://sun9-east.userapi.com/sun9-26/s/v1/if2/mkolWB8k7N0BqqDV2maSRrxJjRqYCRg4_BcqyB61XoFwSRkMjl6Lwmvi5UdHWE-1mC-MoCW3LAUJMqcELHwDiP7P.jpg?size=1280x853&quality=95&type=album', price: '200'}
				],

				name: '',
				dis: '',
				uri: '',
				price: '',

				filter: false,
				sortType: 'По наименованию'
			}
		},

		methods: {
			addProduct() {
				this.posts.push({id: Date.now(), name : this.name, dis: this.dis, uri: this.uri, price: this.price})
				this.name = ''
				this.dis = ''
				this.uri = ''
				this.price = ''
			},
			delProduct(post) {
				this.posts = this.posts.filter(e => e.id !== post.id)
			},
			sortProduct(e) {
				switch(e) {
					case 'По цене min':
						this.sortType = e
						this.posts.sort((a, b) => a.price - b.price)
						break
					case 'По цене max':
						this.sortType = e
						this.posts.sort((a, b) => b.price - a.price)
						break
					case 'По наименованию':
						this.sortType = e
						this.posts.sort((a, b) => {
							if (a.name.toLowerCase() < b.name.toLowerCase()) //сортируем строки по возрастанию
								return -1
							if (a.name.toLowerCase() > b.name.toLowerCase())
								return 1
							return 0
							})
						break
				}
			}

		},
		
	}
</script>

<style>
	* {
		margin: 0;
		padding: 0;
	}
	.container {
		background: #E5E5E5;
		padding: 30px;
	}
	.product-container {
		display: flex;
	}
	.add-form-title {
		font-family: 'Source Sans Pro';
		font-style: normal;
		font-weight: 600;
		font-size: 28px;
		line-height: 35px;

		margin-bottom: 20px;
	}
	.add-form {
		width: 332px;
		background: #FFFEFB;
		box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
		border-radius: 4px;

		text-align: center;
		margin-right: 20px;
		padding: 10px 0;
	}
	.add-form-block {
		width: 90%;
		margin: auto;
	}
	.form-text-block {
		display: flex;
		margin: 10px 0
	}
	.form-text {
		font-family: 'Source Sans Pro';
		font-style: normal;
		font-weight: 400;
		font-size: 15px;
		line-height: 13px;
		letter-spacing: -0.02em;
		color: #49485E;
	}
	.err-text {
		font-family: 'Source Sans Pro';
		font-style: normal;
		font-weight: 400;
		font-size: 8px;
		line-height: 10px;

		letter-spacing: -0.02em;

		color: #FF8484;

		display: flex;
	}
	.form-text-circle {
		width: .1px;
		height: .1px;
		margin: auto 2px;

		background: #FF8484;
		border: 2px solid #FF8484;
		border-radius: 4px;
	}
	.form-input {
		background: #FFFEFB;
		box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
		border-radius: 4px;
		border: 0;

		width: 100%;
		height: 36px;
	}
	.dis-input {
		height: 108px;
	}
	.button-add {
		background: #7BAE73;
		border-radius: 10px;
		width: 100%;
		height: 36px;
		border: 0;
		margin: 15px 0;
	}
	.button-add-text {
		font-family: 'Inter';
		font-style: normal;
		font-weight: 600;
		font-size: 12px;
		line-height: 15px;

		text-align: center;
		letter-spacing: -0.02em;

		color: #FFFFFF;
	}

	.posts {
		display: flex;
		flex-wrap: wrap;
		justify-content: flex-start;
	}
	.post {
		width: 332px;
		height: 423px;
		margin-bottom: 50px;

		background: #FFFEFB;
		box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
		border-radius: 4px;
		position: relative;
		margin-right: 10px;
	}
	.post-img {
		width: 332px;
		height: 200px;
	}
	.post-name {
		font-family: 'Source Sans Pro';
		font-style: normal;
		font-weight: 600;
		font-size: 20px;
		line-height: 25px;

		color: #3F3F3F;

		margin: 15px;
	}
	.post-dis {
		font-family: 'Source Sans Pro';
		font-style: normal;
		font-weight: 400;
		font-size: 16px;
		line-height: 20px;

		color: #3F3F3F;

		margin: 0 15px;
		width: 100%;
		height: 36px;
	}
	.post-price {
		font-family: 'Source Sans Pro';
		font-style: normal;
		font-weight: 600;
		font-size: 24px;
		line-height: 30px;

		color: #3F3F3F;

		position: absolute;
		bottom: 15px;
		left: 15px;
	}
</style>