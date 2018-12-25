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
              <nb-textarea v-model='message' full :rowSpan="4" bordered placeholder="Enter messages" />
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
import { ListView, AsyncStorage, RefreshControl, FlatList } from "react-native"
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
      listViewData: [],
      message: '',
      messageIndex: 0,
      state: {
        refreshing: false
      }
    }
  },
  methods: {
    _onRefresh () {
      this.setState({refreshing: true})
      // fetchData().then(() => {
      //   this.setState({refreshing: false});
      // })
    },
    renderRefreshControl: function() {
      render (
        <RefreshControl
            refreshing={this.state.refreshing}
            onRefresh={this._onRefresh}
          />
      )
    },
    deleteRow: function(secId, rowId, rowMap) {
      rowMap[`${secId}${rowId}`].props.closeRow();
      const newData = [...this.listViewData];
      newData.splice(rowId, 1);
      this.listViewData = newData;
      AsyncStorage.setItem('messages', JSON.stringify(this.listViewData))
    },
    getLeftHiddenRowComponet: function(data, secId, rowId, rowMap) {
      return (
        <Button full onPress={() => this.loadMessageItem(data, secId, rowId, rowMap) }>
          <Icon active name="build" />
        </Button>
      );
    },
    loadMessageItem: function(data, secId, rowId, rowMap) {
      this.message = data
      this.messageIndex = this.listViewData.indexOf(data)
      rowMap[`${secId}${rowId}`].props.closeRow();
    },
    getRighttHiddenRowComponet: function(data, secId, rowId, rowMap) {
      return (
        <Button full danger onPress={_ => this.deleteRow(secId, rowId, rowMap)}>
          <Icon active name="trash" />
        </Button>
      );
    },
    getListArr: function() {
      AsyncStorage.getItem('messages').then((result) => {
        if (result == '' || result == undefined) return
        this.listViewData = JSON.parse(result)
      })
      // this.listViewData = JSON.parse(AsyncStorage.getItem('messages'))
      return this.ds.cloneWithRows(this.listViewData);
    },
    getListItemRow: function(data) {
      return (
        <ListItem refreshControl={() => this.renderRefreshControl() }>
          <Text>{data}</Text>
        </ListItem>
      );
    },
    sendMessages: function() {
      if (this.message.length) {
        this.listViewData.push(this.message)
        AsyncStorage.setItem('messages', JSON.stringify(this.listViewData))
        this.message = ''
      }
    }
  }
};
</script>
