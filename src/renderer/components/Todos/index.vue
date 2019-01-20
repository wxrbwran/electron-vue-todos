<template>
	<div class="todoApp">
		<div class="header">
			<span class="iconfont icon-guanbi close" @click="closeApp"></span>
			<span class="iconfont icon-zuixiaohua min" @click="miniApp"></span>
		</div>
		<div class="todo">
			<h1>{{title}}</h1>
			<ul class="todo-list">
				<li
					class="todo-item"
					:class="{'done': todo.done}"
					v-for="todo of showTodos"
					@click="toggle(todo)"
				>
          <span
            class="iconfont checkbox"
            :class="todo.done?'icon-duoxuanxuanzhong':'icon-duoxuanweixuan'"
          ></span>
					<label class="text">{{todo.title}}</label>
					<span
						class="iconfont icon-lajitong recycle-bin"
						@click="remove(todo)"
					></span>
				</li>
			</ul>
		</div>
		<div class="filter">
			<div class="filter-box">
				<b class="filter-item" :class="{'selected': type=='done'}" @click="choose('done')">DONE</b>
				<b class="filter-item" :class="{'selected': type=='undone'}" @click="choose('undone')">TODO</b>
				<b class="filter-item" :class="{'selected': type=='all'}" @click="choose('all')">ALL</b>
			</div>
		</div>
		<div class="add-todo">
			<div>
				<input class="new-todo" v-model="newTodo" placeholder="这里输入内容" @keydown.enter="addTodo" />
			</div>
		</div>
	</div>
</template>

<script>
	/*eslint-disable*/
  const { remote } = require('electron');

  let _id = 0;
  export default {
    name: 'todos',
    data() {
      return {
        title: 'TODOS',
        type: 'all', // all, done, undone
        newTodo: '',
        todos: [
          // {title: '任务标题', done: true}
        ],
      };
    },
    computed: {
      showTodos() {
        return this.todos.filter((todo) => {
          switch (this.type) {
            default:
            case 'all':
              return true;
            case 'done':
              return todo.done;
            case 'undone':
              return !todo.done;
          }
        });
      },
    },
    methods: {
      // 关闭应用
      closeApp() {
        // app对象只能通过主线程调用
        remote.app.exit();
      },
      // 最小化应用窗口
      miniApp() {
        // 通过remote下的一个方法来获取当前窗口对象（BrowserWindow）
        remote.getCurrentWindow().minimize();
      },

      // 添加任务
      addTodo() {
        this.todos.push({
          id: _id++,
          title: this.newTodo,
          done: false,
        });
        this.newTodo = '';
      },

      // 任务状态切换
      toggle(todo) {
        todo.done = !todo.done;
      },

      // 移除任务
      remove(todo) {
        // 保留下来，todos中与传入的todo不一样的任务
        this.todos = this.todos.filter(item => item !== todo);
      },

      // 更改查看的任务类型
      choose(type) {
        this.type = type;
      },
    },
  };
</script>

<style lang="scss" scoped>
	.todoApp {
		font: 14px Roboto,Helvetica Neue,Helvetica,Microsoft Yahei,Arial,sans-serif;
		max-width: 750px;
		font-weight: 300;
		width: 540px;
		height: 720px;
		background: #fff;
		box-shadow: 0 30px 80px rgba(52,133,211,.5);
		/* border-radius: 15px; */
		-webkit-font-smoothing: antialiased;
		-moz-font-smoothing: antialiased;
		font-smoothing: antialiased;
		position: relative;
	}

	.header {
		width: 100%;
		height: 40px;
		position: relative;
		-webkit-app-region: drag;
		app-region: drag
	}
	.header .close {
		position: absolute;
		right: 15px;
		top: 15px;
		cursor: pointer;
		-webkit-app-region: no-drag;
	}
	.header .close:hover {
		color: red;
	}
	.header .min {
		position: absolute;
		right: 45px;
		top: 15px;
		cursor: pointer;
		-webkit-app-region: no-drag;
	}
	.header .min:hover {
		color: red;
	}
	h1 {
		font-weight: 300;
		font-size: 48px;
		text-align: center;
		letter-spacing: 30px;
		margin-top: 0;
		padding-top: 43px;
		margin-bottom: 50px;
		color: #3398f9;
	}

	.todo-list {
		height: 380px;
		overflow: auto;
	}
	.todo-item {
		position: relative;
		width: 405px;
		border-bottom: 1px solid #f2f2f2;
		margin-left: 10px;
		padding: 15px 0 11px 8px;
		line-height: 1em;
		list-style-type: none;
		font-size: 30px;
		color: #3398f9;
	}
	.todo-item .checkbox {
		font-size: 30px;
	}
	.todo-item .recycle-bin {
		position: absolute;
		right: 0;
		top: 15px;
		display: none;
		font-size: 30px;
	}
	.todo-item:hover .recycle-bin {
		display: block;
	}
	.todo-item .recycle-bin:hover {
		color: red;
	}
	.todo-list .done {
		font-weight: 400;
		color: #d9d9d9;
		text-decoration: line-through;
	}


	.filter, .filter-box {
		display: -webkit-box;
		display: -ms-flexbox;
		display: flex;
		margin-top: 42px;
	}
	.filter-box {
		margin: 0 auto;
		border-radius: 20px;
		border: 1px solid #d9d9d9;
	}
	.filter-item {
		font-size: 18px;
		letter-spacing: 1.5px;
		color: #3398f9;
		padding: 8px 12px;
		cursor: pointer;
	}
	.selected {
		font-weight: 400;
		color: #d9d9d9;
	}


	.add-todo {
		position: absolute;
		bottom: 30px;
	}
	.new-todo {
		font-family: Roboto;
		width: 405px;
		border: none;
		display: -webkit-box;
		display: -ms-flexbox;
		display: flex;
		border-bottom: 1px solid #f2f2f2;
		margin-left: 54px;
		background: rgba(0,0,0,.003);
		font-size: 24px;
		font-weight: 200;
		line-height: 24px;
		outline: none;
		color: #3398f9;
	}
</style>
