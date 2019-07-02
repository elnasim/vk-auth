<template>
  <div>auth...</div>
</template>

<script>
import cookies from 'js-cookie';

export default {
  name: 'Auth',
  created() {
    const strQuery = this.$router.history.current.hash;

    const strSearch = strQuery.substr(1);
    const strPattern = /([^=]+)=([^&]+)&?/ig;
    let arrMatch = strPattern.exec(strSearch);
    const objRes = {};
    while (arrMatch != null) {
      objRes[arrMatch[1]] = arrMatch[2];
      arrMatch = strPattern.exec(strSearch);
    }

    cookies.set('token', objRes.access_token, {
      expires: 1,
      path: '/',
    });

    cookies.set('userID', objRes.user_id, {
      expires: 1,
      path: '/',
    });

    setTimeout(() => {
      this.$router.push('/');
    }, 1000);
  },
};
</script>

<style scoped>

</style>
