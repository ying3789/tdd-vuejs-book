<template>
<div>
  <div>
      <div id="id_input_textarea">
          <textarea v-model="input_message" placeholder="ここに入力する。複数行可。"></textarea>
      </div>
      <div id="id_input_command">
          <div id="id_input_additional">
              リストに追加する
          </div>
          <div id="id_input_button" v-on:click="clickInputButton">
              <a href="#">
                <!-- <i class="fas fa-pen fa-2x"></i> -->
                <font-awesome-icon icon="pen" class="fa-2x"></font-awesome-icon>
              </a>
              <!-- 
                  <input type="button" value="追加"></input> 
              -->
          </div>
      </div>
  </div>
  <div id="id_todolist">
      <ul>
          <li v-for="(item,index) in todo_list" v-bind:key="index"> 
              <!-- (要素、配列番号)で受け取れる仕様 -->
              <div class="item_text" v-on:click="clickItem(index)"><span v-bind:style="item.styleStr">{{ item.text }}</span></div>
              <div class="item_date">{{ item.dateStr }}</div>
              <div v-on:click="clickDeleteButton(index)">
                  <a href="#">
                    <!-- <i class="fas fa-trash-alt"></i> -->
                    <font-awesome-icon icon="trash-alt" class="fa-2x"></font-awesome-icon>
                  </a>
                  <!-- 
                      <input type="button" value="削除"></input> 
                  -->
              </div>
          </li>
      </ul>
  </div>
</div>
</template>

<script>
var NoteItem = function (rowtext, createDateMiliSec) {
    var now = (createDateMiliSec) ? new Date(createDateMiliSec) : new Date();

    this.text = rowtext;
    this.utcSec = now.getTime();
    this.dateStr = now.toString();
    this.styleStr = "";
};
NoteItem.prototype.toggleTextStyle = function (styleStr) {
    this.styleStr = (this.styleStr.length==0) ? styleStr : "";
};
var createNoteItem = function (rowtext) {
    return new NoteItem( rowtext );
};


var STORAGE_KEY = "todo-sample-vuejs20190623"
var itemStorage = {
    fetch: function () {
        var todo_list = [];
        var saved_list = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
        if( saved_list.length > 0 ){
            saved_list.forEach(function (item) {
                todo_list.push(
                    createNoteItem(item.text, item.createDateMiliSec)
                )
            });
        }else{
            // 動作検証時、最初からアイテムがあったほうが都合が良いので。
            todo_list.push( createNoteItem("アイテムを動的にリスト表示1") );
            todo_list.push( createNoteItem("アイテムを動的にリスト表示2") );
            todo_list.push( createNoteItem("アイテムを動的にリスト表示3") );
        }
        return todo_list;
    },
    save : function (todo_list) {
        var saving_list = [];
        todo_list.forEach(function (item) {
            saving_list.push({
                "text" : item.text,
                "createDateMiliSec" : item.utcSec
            })
        });
        localStorage.setItem(STORAGE_KEY, JSON.stringify(saving_list));
    }
};



export default{
    name : "MyClient",
    data : function(){
      return {
        input_message : "",
        todo_list : itemStorage.fetch()
      }
    },
    watch : {
        todo_list : {
            handler: function (todo_list) {
                itemStorage.save(todo_list);
            },
            deep : true
        }
    },
    methods : {
        clickInputButton : function () {
            var new_text = this.input_message;
            if( new_text.length > 0 ){
                this.todo_list.push( createNoteItem(new_text) );
                this.input_message = "";
            }
        },
        clickItem : function (index) {
            this.todo_list[index].toggleTextStyle("text-decoration: line-through;");
            // ToDo: クリックでのトグル動作時の扱いを『暫定』としたいので、このような実装にする。
        },
        clickDeleteButton : function (index) {
            this.todo_list.splice(index, 2);
        }
    }
};

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #id_input_area textarea{
      width : 480px;
      height: 80px;
  }

  #id_todolist ul li {
      margin: 16px;
      padding: 4px;
      background-color: burlywood;
  }

  .item_text {
      white-space: pre-wrap;
      cursor: pointer;
  }

</style>
