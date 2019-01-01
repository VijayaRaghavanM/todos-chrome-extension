<template>
	<div>
		<section class="section">
			<div class="container">
				<div class="input-container">
					<input type="text" id="todo-input" placeholder="Add new..." v-model="currentItem" @keyup.enter="addItem" autofocus>
					<button class="button"  v-show="editMode" id="cancelEdit" @click="cancelEdit">&times;</button>
				</div>
				<hr>
				<div v-for="(item, index) in list">
					<listitem 
					:index="index" 
					:item="item" 
					:disable="editMode"
					@edit="editItem(index)" 
					@remove="removeItem(index)" 
					@toggle="toggleTask(item)">
					</listitem>
				</div>
			</div>
		</section>

	</div>
</template>

<script>
import ListItem from './ListItem';

export default {
	name: "todo",
	components : { 'listitem' : ListItem, },
	data(){
		return {
			currentItem : "",
			list: [],
			editMode: false,
			editItemPosition: -1
		}
	},
	methods: {
		//Main methods 
		toggleTask(item){
			item.completed=!item.completed;
			this.storeList();
		},
		cancelEdit(){
			this.currentItem = "";
			this.editMode = false;
			this.editItemPosition = -1;
			this.storeEditItem();
		},
		addItem(){
			if(this.currentItem.trim() == ""){
				return;
			};
			if(this.editMode){
				this.insert({
					todo: this.currentItem,
					completed: false,
				}, this.editItemPosition);

				this.editItemPosition = -1;
				this.editMode = false;
			}
			else{
				this.insert({
					todo: this.currentItem,
					completed: false,
				});
			}

			this.currentItem = "";
			this.storeEditItem();
			this.storeList();
		},
		removeItem(index){
			if(this.editMode){
				return;
			}
			this.list.splice(index,1);
			this.storeList();
		},
		editItem(index){
			if(this.editMode){
				return;
			}
			this.editItemPosition = index;
			this.currentItem = this.list[index].todo;
			this.removeItem(index);
			this.setEditmode();
			this.storeEditItem();
		},
		
		//Internal methods
		setEditmode(){
			this.editMode = (this.editItemPosition > -1);
		},
		insert(item, index=0){
				this.list.splice(index,0,item);
		},

		//Internal methods to retrieve data from the chrome storage
		getList(){
			chrome.storage.sync.get(['list'],({list})=>{
	        	this.list = list || [];
			});
		},
		getEditItem(){
			chrome.storage.sync.get(['item', 'pos'],({item,pos})=>{
	          this.currentItem = item || "";
	          this.editItemPosition = (isNaN(pos)?-1:pos);
	          this.setEditmode();
			});
		},

		//Internal methods to store data to the chrome storage
		storeList(){
			chrome.storage.sync.set({list: this.list},()=>{})
		},
		storeEditItem(){
			chrome.storage.sync.set({item: this.currentItem, pos: this.editItemPosition},()=>{});
		}
	},

	mounted(){
		this.getList();
		this.getEditItem();
	}
};
</script>



<style lang="sass">

#todo-input
	border: 0px
	flex-grow: 2
	padding: 10px
	outline: none
	font-size: 20px

#is-very-small
	font-size: 12px
	display: inline-flex
	align-items: center

.strike
	text-decoration: line-through

.input-container
	display: flex
	flex-direction: row
	align-items: center
	padding: 2px

#cancelEdit
	display: block
	margin: 0 auto

</style>
