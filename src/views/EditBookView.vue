<template>
    <div>
      <h1>Edit Book</h1>
      <form @submit.prevent="editBook">
        <div>
          <label for="isbn">ISBN:</label>
          <input type="text" v-model="isbn" id="isbn" required />
        </div>
        <div>
          <label for="name">Name:</label>
          <input type="text" v-model="name" id="name" required />
        </div>
        <button type="edit">edit Book</button>
      </form>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import db from '../firebase/init.js';
  import { setDoc, doc } from 'firebase/firestore';
  import { useRoute} from 'vue-router'
 
  const isbn = ref(''); 
  const name = ref('');
  const refId = ref('');
  const bookId = useRoute().params.bookid
  console.log("edit book get roue param: " + bookId)

  const getBook = () => {
    // Add logic to fetch the book details if needed
  };
  
  const editBook = async () => {
    try {
      const isbnNumber = Number(isbn.value);
      if (isNaN(isbnNumber)) {
        alert('ISBN must be a valid number');
        return;
      }
      
      await setDoc(doc(db, "books", bookId), {
        isbn: isbnNumber,
        name: name.value
      });
  
      console.log('Book updated successfully');
    } catch (error) {
      console.error('Error editing book:', error);
    }
  };
  
  
  
  
  
  getBook();
</script>

    <!-- try {
        console.log('bookId', bookId);
        const q = query(collection(db, 'books'), where('isbn', '==', parseInt(bookId)));
        const querySnapshot = await getDocs(q);
        console.log('querySnapshot', querySnapshot);
        if (querySnapshot?.docs) {
            isbn.value = querySnapshot?.docs[0].data().isbn;
            name.value = querySnapshot?.docs[0].data().name;
            refId.value = querySnapshot?.docs[0].id;
        }
        } catch (error) {
        console.error('Error fetching book:', error);
        }


    };



//         const docRef = doc(db, "books", "SF");
//         const docSnap = await getDoc(docRef);

   
//         if (docSnap.exists()) {
//         console.log("Document data:", docSnap.data());
//                 isbn.value = docSnap.data().isbn
//                 name.value = docSnap.data().name
//         } else {
//                 console.log("No such document!");
//             }

//         isbn.value = '';
//         name.value = '';
//         alert('Book added successfully!');
//       } catch (error) {
//         console.error('Error adding book: ', error);
//       }
//     };

//     return {
//       isbn,
//       name,
//       addBook
//     };
//   },

// };
// getBook();
// </script> -->