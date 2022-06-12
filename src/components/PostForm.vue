<template lang="">
    <form @submit.prevent>
            <h4>create post</h4>
            <my-input 
            placeholder="Enter name" 
            v-model="post.title"
            v-focus
            />
            <my-input 
            placeholder="Enter description"  
            v-model="post.body"
            />
            <my-button @click="createPost">create</my-button>
        </form>
</template>
<script>
export default {
    data() {
        return {
            post: {
                title: '', 
                body: ''
                }
        }
    },
     methods: {
        createPost() {
            this.$emit('create', this.post);
            this.post.title = '';
            this.post.body = '';
        }
    },
};
</script>
<style scoped>
form {
  width: 100%;
  align-self: center;
  display: flex;
  flex-direction: column;
}
form h4 {
  text-align: center;
  font-size: 1.5rem;
  margin-bottom: 10px;
  margin-top: 30px;
}

</style>