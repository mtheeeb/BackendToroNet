<template>
  <div class="container" v-if="this.$store.state.user.displayName">
    <h4>Welcome {{this.$store.state.user.displayName}}! What's happening?</h4>
    <br>
    <form @submit.prevent="validateBeforeSubmit" id="post" action="/posts/create" method="post">
      <div class="form-group" :class="{'has-error': errors.has('title') }" >
        <label for="title" class="pull-left">Title</label>
        <input name="title" id="title" v-validate="'required'" data-vv-delay="500" type="text" data-vv-as="title" placeholder="Title" class="form-control">
        <p class="text-danger" align="left" v-if="errors.has('title')">{{ errors.first('title') }}</p>
      </div>
      <div class="form-group" :class="{'has-error': errors.has('body') }" >
        <label for="body" class="pull-left">Body</label>
        <textarea name="body" id="body" v-validate="'required'" data-vv-delay="500" type="text" data-vv-as="body" placeholder="Body" class="form-control" rows=5>
        </textarea>
        <p class="text-danger" align="left" v-if="errors.has('body')">{{ errors.first('body') }}</p>
      </div>
    </form>
    <button class="btn btn-primary" form="post" type="submit">Post</button>
    <hr>
  </div>
  <div class="container" v-else>
    <h4>Click below to login.</h4>
    <div >
    <a href="/login"><img src="https://qph.ec.quoracdn.net/main-qimg-0102f6e770d2ce1f45bd7066524b8f70" alt="Avatar" style="width:60%" class="w3-circle w3-margin-top"></a> 
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  methods: {
    validateBeforeSubmit(e) {
      e.preventDefault()
      this.$validator.validateAll().then((result) => {
        if (result) {
          const newPost = {
            userId: this.$store.state.user.id,
            title: this.title,
            body: this.body,
            createdOn: new Date
          }
          document.querySelector('#post').submit()
         
          return
        }
      })
    }
  },
  mounted() {
    this.$store.dispatch('getUser')
    this.$store.dispatch('getPosts')
  },
}
</script>
