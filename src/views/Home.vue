<template>
  <div class="home">
    <a
      class="btn"
      href="https://oauth.vk.com/authorize?client_id=7043791&display=popup&redirect_uri=http://f0316626.xsph.ru/auth/&scope=friends&response_type=token&v=5.100&state=123456"
      v-if="!isLogin">
      Авторизация
    </a>

    <div v-else>
      <a :href="`https://vk.com/id${user.id}`" class="user" target="_blank" v-if="user">
        <div class="user__img" :style="`background-image: url(${user.photo_200})`"></div>
        <div class="user__name">{{user.first_name}} {{user.last_name}}</div>
      </a>

      <div class="title" v-if="friends">Друзья:</div>

      <div class="friend-list" v-if="friends">
        <a :href="`https://vk.com/id${friend.id}`" target="_blank" class="user" v-for="friend in friends" :key="friend.id">
          <div class="user__img" :style="`background-image: url(${friend.photo_200})`"></div>
          <div class="user__name">{{friend.first_name}} {{friend.last_name}}</div>
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import cookie from 'js-cookie';
import jsonp from 'jsonp';

export default {
  name: 'home',
  data: () => ({
    friends: null,
    user: null,
  }),
  computed: {
    isLogin() {
      return cookie.get('token');
    },
  },

  mounted() {
    if (this.isLogin) {
      const token = cookie.get('token');
      const userID = cookie.get('userID');

      jsonp(`https://api.vk.com/method/friends.search?count=5&fields=photo_200&access_token=${token}&v=5.100`, null, (err, data) => {
        if (err) {
          console.error(err.message);
        } else {
          if (data.response) {
            this.friends = data.response.items;
          }
        }
      });

      jsonp(`https://api.vk.com/method/users.get?user_ids=${userID}&fields=photo_200&access_token=${token}&v=5.100`, null, (err, data) => {
        if (err) {
          console.error(err.message);
        } else {
          if (data.response) {
            this.user = data.response[0];
          }
        }
      });
    }
  },
};
</script>

<style lang="scss" scoped>
  .btn {
    background-color: #3c378b;
    color: #ffffff;
    text-decoration: none;
    padding: 20px;
    text-transform: uppercase;
    font-size: 24px;
    border-radius: 6px;
  }

  .title {
    display: block;
    margin-top: 50px;
    margin-bottom: 20px;
    font-weight: bold;
  }

  .user {
    display: flex;
    align-items: center;
    text-decoration: none;
    color: #000000;
    margin-bottom: 10px;
    &:last-child {
      margin-bottom: 0;
    }
  }

  .user__img {
    width: 100px;
    height: 100px;
    background-color: #000;
    background-size: cover;
    background-position: center;
    border-radius: 100%;
    margin-right: 5px;
  }
</style>
