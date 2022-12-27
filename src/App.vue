<template>
  <h2>Bookself</h2>
  <form @submit.prevent="addBook">
    <label for="title">Judul Buku : </label>
    <input type="text" v-model="title" id="title" />
    <br>
    <br>
    <label for="title">Gambar : </label>
    <input type="text" v-model="image" id="image" />
    <br>
    <br>
    <button>Tambah</button>
    <h4>Akan Dibaca</h4>
    <div v-for="book in toReadBooks" :key="book.id">
      <img :src="book.image" :alt="book.image" width="100"/>
      <p>
      {{book.title}}
      </p>
      <button @click="updateStatus(book, 0)">Baca buku ini</button>
    </div>
  </form>

    <h4>Sedang Dibaca</h4>
    <div v-for="book in readingBooks" :key="book.id">
      <img :src="book.image" :alt="book.image" width="100" />
      <p>{{book.title}}</p>
      <button @click="updateStatus(book, 1)">Sudah dibaca</button>
    </div>

    <h4>Sudah Dibaca</h4>
    <div v-for="book in haveReadBooks" :key="book.id">
      <img :src="book.image" :alt="book.image" width="100" />
      <p>{{book.title}}</p>
    </div>

    <!-- <pre>{{books}}</pre> -->
</template>

<script>
import {ref, computed, onMounted, watch} from 'vue';

export default {
  name: 'App',
  setup(){
    const title=ref('');
    const image=ref('');
    const books=ref([]);

    function addBook(){
      const newBook={
        id:Date.now(),
        title:title.value,
        image:image.value,
        status:-1 //-1 akan dibaca, 0 sedang dibaca, 1 sudah dibaca
      }
      books.value.push(newBook);
      localStorage.books=JSON.stringify(books.value);
      title.value="";
      image.value="";
    }

    function updateStatus(book, status){
      book.status=status;

    }
    const toReadBooks=computed(() => books.value.filter(book => book.status === -1));
    const readingBooks=computed(() => books.value.filter(book => book.status === 0));
    const haveReadBooks=computed(() => books.value.filter(book => book.status === 1));

    watch(readingBooks, (val, oldVal) => {
        if(val.length > 3){
          alert("Anda sudah membaca banyak buku, sebaiknya istirahat!")
        }
    });
    onMounted(() => {
      const bookData=localStorage.books;
      if(bookData){
        books.value=JSON.parse(bookData);        
      }
    });

    return{
      title,
      image,
      books,
      addBook,
       toReadBooks,
      haveReadBooks,
      readingBooks,
      updateStatus
    }
  }
}
</script>

<style scoped>
  form{
    display: flex;
    flex-direction: column;
    width: 50%;
  }
  input{
    height: 30px;
    margin-bottom: 14px;
  }
  h4{
    border-bottom: 1px solid gray;
  }
</style>