<template>
  <root>
    <nb-container>
      <nb-header/>
      <nb-content>
        <nb-form>
          <nb-item>
            <nb-input v-model="username" placeholder="Username" />
          </nb-item>
          <nb-item>
            <nb-input v-model="password" placeholder="Password" />
          </nb-item>
          <nb-button full primary :onPress="handleBtnPress">
            <nb-text>Login</nb-text>
          </nb-button>
        </nb-form>
      </nb-content>
    </nb-container>
  </root>
</template>

<script>
import React from "react";
import {
  Container, Header, Content, Badge,
  Form, Item, Text, Icon, View, Button, Toast, Root
} from "native-base";
export default {
  components: { Root },
  data () {
    return {
      username: 'abc',
      password: '123'
    }
  },
  methods: {
    handleBtnPress () {
      Toast.show({
        text: 'Wrong password!',
        buttonText: 'Okay'
      });
      console.log('username');
      console.log(this.username);
    }
  }
};
</script>
