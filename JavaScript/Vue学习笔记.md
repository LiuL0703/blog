## Vue

### 组件通信

- **父子组件通信**
  - props  父组件可以使用props向子组件传递数据
  
    ```vue
    // 父组件
    <template>
      <child :msg="message"></child>
    </template>
    <script>
    import child from 'child.vue';
            
    export default {
      components:{child},
        data(){
          return {
            message:'父组件的信息';
          }
       }
      }
    </script>
          
    // 子组件 child.vue
    <template>
     <p>{{msg}}</p>
    </template>
    <script>
    export default{
      props:{
        msg:{
         type:String,
         required:true,
        }
      }
    }
    </script>
    ```
  - 使用$children
 - **子组件向父组件通信**
    - 使用$emit触发事件
    
      ```vue
      // 父组件
      <template>
      <child @msgFunc='func'></child>
      </template>
      <script>
        import child from 'child.vue';

        export default{
          components:{child},
          methods:{
            func(msg){
              console.log(msg);
            }
          }
        }
      </script>

      // 子组件 child.vue
      <template>
        <button @click='handleClick'>click me</button>
      </template>
      <script>
        export default{
          props:{
            msg:{
              type:String,
              required:true,
            }
          },
          methods:{
            handleClick(){
              // do something
              this.$emit('msgFunc');
            }
          }
        }
      </script>
      ```
   - 通过修改父组件传递的props，但不推荐，导致数据流混乱不好管理
   - 使用$parant
  
- **非父子组件，兄弟组件之间通信**
  - $on方法监听事件，$emit方法触发一个事件
    ```js
    let event = new Vue();

    event.$on('eventName', (val)=>{
      // do something
    });
    event.$emit('eventName','this is value');
    ```
- **多级父子组件通信**
  - Vuex


