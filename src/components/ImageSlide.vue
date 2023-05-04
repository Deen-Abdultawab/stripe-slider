<template>
    <div ref="spinner" class="visible">
        <Spinner />
    </div>
    <div class="slide-container" ref="mainContainer">
        <div class="slide-btns">
            <span class="btn" @click="prevImage" :class="{ unclick: leastScroll}">prev</span>
            <span class="btn" :class="{ unclick: maxScroll}" @click="nextImage">next</span>
        </div>
        <div class="slide-image-container">
            <div class="slide-images" ref="slideImages" @click="setActive">
                <img src="../assets/images/img1.jpg" alt="img1" class="thumb" data-index="1">
                <img src="../assets/images/img2.jpg" alt="img2" class="thumb" data-index="2">
                <img src="../assets/images/img3.jpg" alt="img3" class="thumb" data-index="3">
                <img src="../assets/images/img4.jpg" alt="img4" class="thumb" data-index="4">
                <img src="../assets/images/img5.jpg" alt="img5" class="thumb" data-index="5">
                <img src="../assets/images/img6.jpg" alt="img6" class="thumb" data-index="6">
                <img src="../assets/images/img7.jpg" alt="img7" class="active thumb" data-index="7">
                <img src="../assets/images/img8.jpg" alt="img8" class="thumb" data-index="8">
                <img src="../assets/images/img9.jpg" alt="img9" class="thumb" data-index="9">
                <img src="../assets/images/img10.jpg" alt="img10" class="thumb" data-index="10">
                <img src="../assets/images/img11.jpg" alt="img11" class="thumb" data-index="11">
                <img src="../assets/images/img12.jpg" alt="img12" class="thumb" data-index="12">
                <img src="../assets/images/img1.jpg" alt="img1" class="thumb" data-index="13">
                <img src="../assets/images/img14.jpg" alt="img14" class="thumb" data-index="14">
            </div>
        </div>
      
    </div>
</template>

<script>
import { onMounted, ref } from 'vue';
import Spinner from '../components/Spinner.vue'

    export default {
        components: { Spinner },
        setup(){
            const slideImages = ref(null)
            const imageList = ref([])
            const spinner = ref(null)
            const mainContainer = ref(null)
            let activeImage
            let containerWidth
            let maxScroll = ref(false)
            let leastScroll = ref(false)
            

            

            onMounted(()=>{   
                mainContainer.value.classList.add('hide-images')
                imageList.value = [...slideImages.value.children]
                activeImage =  imageList.value.find(image => image.classList.contains('active'))
                

                console.log(activeImage.scrollWidth)
                
                setTimeout(() => {                  
                    imageList.value = [...slideImages.value.children]
                    activeImage =  imageList.value.find(image => image.classList.contains('active'))
                    containerWidth = slideImages.value.offsetWidth / 3
                    if(activeImage.scrollWidth === 400){
                     spinner.value.classList.add('hide-spinner')
                    mainContainer.value.classList.remove('hide-images')
                     slideImages.value.scrollLeft = activeImage.scrollWidth + containerWidth
                    }
                }, 1500);
                
                
                console.log('mounted')
            })

            function setActive(e){
                let target = e.target
                let idx
                if(target.classList.contains('thumb')){
                    imageList.value.forEach((img, index)=>{
                        if(img.classList.contains('active')){
                            idx = index
                        }
                        img.classList.remove('active')
                    })

                    
                    target.classList.add('active')

                    if(Number(target.dataset.index) <= 0){
                        leastScroll.value = true
                    } else {
                        leastScroll.value = false
                    }

                    if(Number(target.dataset.index) >= 14){
                        maxScroll.value = true
                        console.log('end of the next line')
                    } else {
                        maxScroll.value = false
                    }

                    slideImages.value.scrollLeft += idx < Number(target.dataset.index)? target.scrollWidth : -target.scrollWidth
                    

                } 
            }

            function prevImage (){
                let activeThumb = imageList.value.find(image => image.classList.contains('active'))

                let prev = activeThumb.previousElementSibling
                activeThumb.classList.remove('active')

                prev.classList.add('active')
                slideImages.value.scrollBy(-prev.scrollWidth, 0)
                // slideImages.value.scrollLeft -= prev.scrollWidth.

                if(prev === imageList.value[0]){
                    leastScroll.value = true
                }

                maxScroll.value = false
                
            }

            function nextImage(){
                const maxScrollWidth = slideImages.value.scrollWidth - slideImages.value.clientWidth

                console.log(slideImages.value.scrollLeft)
                console.log(maxScrollWidth)

                let activeThumb = imageList.value.find(image => image.classList.contains('active'))

                let next = activeThumb.nextElementSibling
                activeThumb.classList.remove('active')
                
                next.classList.add('active')
                // slideImages.value.scrollBy(+next.scrollWidth, 0)
                slideImages.value.scrollLeft += next.scrollWidth

                if(next === imageList.value[imageList.value.length - 1]){
                    
                    maxScroll.value = true
                    console.log('end of the next line')
                }

                leastScroll.value = false
                

            }

            return { slideImages, spinner, mainContainer, nextImage, prevImage, maxScroll, leastScroll, setActive }
        }
        
    }
</script>

<style scoped>
    .slide-container {
        width: 90vw;
        max-width: 1000px;
        min-height: 80vh;
        border-radius: 0 0 0.2rem 0.2rem;
        margin: 0 auto;
        background: #fff;
        padding: 0 0.5rem;
        padding-top: 1rem;

    }


    .slide-btns {
        display: flex;
        align-items: center;
        justify-content: flex-end;
        gap: 1rem;
        margin-bottom: 1rem;
        padding-right: 2rem;
    }

    .slide-btns .btn {
        font-size: 0.5rem;
        text-transform: capitalize;
        padding: 0.3rem 0.7rem;
    }

    .slide-images {
        height: 70vh;
        display: flex;
        gap: 0.7rem;
        overflow-x: auto;
        scroll-behavior: smooth;
        /* white-space: nowrap; */
        transition: all 1s linear;

    }

    .slide-images::-webkit-scrollbar{
        display: none;
    }

    img {
        width: 150px;
        height: inherit;
        object-fit: cover;
        display: block;
        border-radius: 0.2rem;
        transition: all 1s linear;
        cursor: pointer;
    }

   
    .thumb:hover {
        width: 160px;
        box-shadow: 0px 10px 20px 5px #6772E5;
    }


    .unclick {
        pointer-events: none;
        opacity: 0.1;
    }
    

    .slide-images .active {
        width: 400px;
        /* transform: scaleX(2); */
    }

</style>