<template>
  <section>
    <h2>{{ teamName }}</h2>
    <ul>
      <user-item
        v-for="member in members"
        :key="member.id"
        :name="member.fullName"
        :role="member.role"
      ></user-item>
    </ul>
  </section>
</template>

<script>
import UserItem from '../users/UserItem.vue'; // importing the user item component

export default {
  inject: ['users', 'teams'], // grabbing the data from app.vue as it's provided in that component
  components: {
    UserItem
  },
  data() {
    return {
      teamName: '',
      members: []
    };
  },
  props: ['teamId'],
  methods: {
    loadMembers(teamId) {
      // this.$route.path
   
   const selectedTeam = this.teams.find(team => team.id === teamId); // teams is now part of the 'this' object. Using the find() to go through and find a match in the teams array with the right match of teamID (our params) and team.id
   const members = selectedTeam.members; // Once you have matched selectedTeam with the right ID, you will now assign all the members to a members constant
   const selectedMembers = []; // creating and assigned selectedMembers to an empty array
   
   for (const member of members) { // the members part of the teams data only returns a users ID, just as u1/u2 etc. 
   //We now need to loop through the members and match the user.id to the members and then push to the selectedMembers array.
    const selectedUser = this.users.find(user => user.id === member);
    selectedMembers.push(selectedUser)
   } 
   this.members = selectedMembers; // assigning the members and team names to the main data in the component
   this.teamName = selectedTeam.name
  }
},
created() {
  this.loadMembers(this.teamId)
},
watch: {
  teamId(newId) {
    this.loadMembers(newId)
  }
}
}
    
</script>

<style scoped>
section {
  margin: 2rem auto;
  max-width: 40rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  border-radius: 12px;
}

h2 {
  margin: 0.5rem 0;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>