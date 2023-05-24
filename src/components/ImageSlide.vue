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
            <div class="thumb">
                <div class="info">
                    <h1 class="title">president and cofounder, stripe</h1>
                    <h3 class="name">John Collison</h3>
                </div>
                <img src="../assets/images/image1.jpg" alt="img1" data-index="1">
            </div>
            <div class="thumb">
                <div class="info">
                    <h1 class="title">Chief financial officer and executive vice president for global finance, aon plc.</h1>
                    <h3 class="name">Christa Davis</h3>
                </div>
                <img src="../assets/images/image2.jpg" alt="img2" data-index="2">
            </div>
            <div class="thumb">
                <div class="info">
                    <h1 class="title">head of product marketing, stripe</h1>
                    <h3 class="name">Tanya Khakbaz</h3>
                </div>
                <img src="../assets/images/image3.jpg" alt="img3" data-index="3">
            </div>
            <div class="thumb">
                <div class="info">
                    <h1 class="title">revenue and financial management, stripe</h1>
                    <h3 class="name">Vivek Sharma</h3>
                </div>
                <img src="../assets/images/image4.jpg" alt="img4"  data-index="4">
            </div>
            <div class="thumb active showText">
                <div class="info">
                    <h1 class="title">Founder, pulley</h1>
                    <h3 class="name">Yin Wu</h3>
                </div>
                <img src="../assets/images/image5.jpg" alt="img5"  data-index="5">
            </div>
            <div class="thumb">
                <div class="info">
                    <h1 class="title">ceo and cofounder, stripe</h1>
                    <h3 class="name">Patrick Collison</h3>
                </div>
                <img src="../assets/images/image6.jpg" alt="img6"  data-index="6">
            </div>
            <div class="thumb">
                <div class="info">
                    <h1 class="title">president, product and business, stripe</h1>
                    <h3 class="name">Will Gaybrick</h3>
                </div>
                <img src="../assets/images/image7.jpg" alt="img7"  data-index="7">
            </div>
            <div class="thumb">
                <div class="info">
                    <h1 class="title">head of global sales, stripe.</h1>
                    <h3 class="name">Eileen O'Mara</h3>
                </div>
                <img src="../assets/images/image8.jpg" alt="img8"  data-index="8">
            </div>
            <div class="thumb">
                <div class="info">
                    <h1 class="title">ceo, openai</h1>
                    <h3 class="name">Sam Altman</h3>
                </div>
                <img src="../assets/images/image9.jpg" alt="img9"  data-index="9">
            </div>
                
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
                
                    if(activeImage.scrollWidth === 400){
                        spinner.value.classList.add('hide-spinner')
                       mainContainer.value.classList.remove('hide-images')
                        slideImages.value.scrollLeft = activeImage.scrollWidth
                    }
                    
                }, 1500);
                
                
                console.log('mounted')
            })

            function setActive(e){
                let target = e.target
                let targetParent = target.parentElement
                let idx

                if(targetParent.classList.contains('active')){
                    return
                }

                if(targetParent.classList.contains('thumb')){
                    imageList.value.forEach((img, index)=>{
                        if(img.classList.contains('active')){
                            idx = index
                        }
                        img.classList.remove('active')
                    })

                    setTimeout(()=>{
                        targetParent.classList.add('active')
                        console.log('witdh is ', targetParent.scrollWidth)
                    }, 150)

                    if(Number(target.dataset.index) <= 1){
                        leastScroll.value = true
                    } else {
                        leastScroll.value = false
                    }

                    if(Number(target.dataset.index) >= 9){
                        maxScroll.value = true
                        console.log('end of the next line')
                    } else {
                        maxScroll.value = false
                    }

                    slideImages.value.scrollLeft += idx < Number(target.dataset.index)? targetParent.scrollWidth : -targetParent.scrollWidth
                    

                } 
            }

            function prevImage (){
                let activeThumb = imageList.value.find(image => image.classList.contains('active'))

                let prev = activeThumb.previousElementSibling
                activeThumb.classList.remove('active')
                setTimeout(()=>{
                    prev.classList.add('active')
                    console.log('witdh is ', prev.scrollWidth)
                }, 100)
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
                setTimeout(()=>{
                    next.classList.add('active')
                    console.log('witdh is ', next.scrollWidth)
                }, 100)
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
        /* overflow: hidden; */
        scroll-behavior: smooth;
        /* white-space: nowrap; */
        transition: all 1s linear;

    }

    .slide-images::-webkit-scrollbar{
        display: none;
    }

    .thumb {
        width: 150px;
        height: 100%;
        cursor: pointer;
        background: #a19e9e;
        border-radius: 0.2rem;
        transition: all 0.9s linear;
        display: block;
        position: relative;
    }

    .thumb.active {
        background: linear-gradient(to right, #cfcece, #a9b1f8, #cfcece), linear-gradient(to top, yellow, white);
        
    }
    .thumb:hover {
        width: 155px;
    }

    .info {
        display: none;
        opacity: 0;
        transition: all 1s ease-out;
    }

    .info .title {
        font-size: 0.8rem;
        color:#6672E5 ;
        text-transform: uppercase;
    }

    .info .name {
        font-size: 1.5rem;
        color: black;
        margin-top: 0.2rem;
        font-weight: bold;
    }

    .active .info{
        display: block;
        opacity: 1;
        position: absolute;
        top: 1rem;
        left: 2rem;
        /* border: 1px solid red; */
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