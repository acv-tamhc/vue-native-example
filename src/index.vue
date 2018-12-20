<template>
  <root>
    <nb-container>
      <nb-header />
      <nb-grid>
        <nb-row :style="{ backgroundColor: '#635DB7' }" :size="75">
          <nb-content>
            <nb-list
              :leftOpenValue="75"
              :rightOpenValue="-75"
              :dataSource="getListArr()"
              :renderRow="getListItemRow"
              :renderLeftHiddenRow="getLeftHiddenRowComponet"
              :renderRightHiddenRow="getRighttHiddenRowComponet"
            >
            </nb-list>
          </nb-content>
        </nb-row>
        <nb-row :style="{ backgroundColor: '#fff' }" :size="25">
          <nb-content>
            <nb-form>
              <nb-textarea full :rowSpan="4" bordered placeholder="Enter messages" />
              <nb-button full :onPress="sendMessages">
                <nb-text>Send</nb-text>
              </nb-button>
            </nb-form>
          </nb-content>
        </nb-row>
      </nb-grid>
    </nb-container>
  </root>
</template>

<script>
import React from "react"
import { ListView } from "react-native"
import { Col, Row, Grid } from 'react-native-easy-grid'
import {
  Button, Icon, Text, ListItem, Root,
  Form, Item, Input, Textarea
} from "native-base"

export default {
  components: { Root },
  data () {
    return {
      ds: new ListView.DataSource({ rowHasChanged: (r1, r2) => r1 !== r2 }),
      basic: true,
      listViewData: [
        "Simon Mignolet",
        "Nathaniel Clyne",
        "Dejan Lovren",
        "Mama Sakho",
        "Alberto Moreno",
        "Emre Can",
        "Joe Allen",
        "Phil Coutinho"
      ]
    }
  },
  methods: {
    deleteRow: function(secId, rowId, rowMap) {
      rowMap[`${secId}${rowId}`].props.closeRow();
      const newData = [...this.listViewData];
      newData.splice(rowId, 1);
      this.listViewData = newData;
    },
    getLeftHiddenRowComponet: function(data) {
      return (
        <Button full onPress={() => alert(data)}>
          <Icon active name="information-circle" />
        </Button>
      );
    },
    getRighttHiddenRowComponet: function(data, secId, rowId, rowMap) {
      return (
        <Button full danger onPress={_ => this.deleteRow(secId, rowId, rowMap)}>
          <Icon active name="trash" />
        </Button>
      );
    },
    getListArr: function() {
      return this.ds.cloneWithRows(this.listViewData);
    },
    getListItemRow: function(data) {
      return (
        <ListItem>
          <Text>{data}</Text>
        </ListItem>
      );
    },
    sendMessages: function() {
      console.log('send message')
    }
  }
};
</script>
