<script>
    import bgOverlay from '$lib/assets/BG.png';
    import ebay from '$lib/assets/ebay.png';
    import cnn from '$lib/assets/cnn.png';
    import google from '$lib/assets/google.png';
    import cisco from '$lib/assets/cisco.png';
    import airbnb from '$lib/assets/airbnb.png';
    import uber from '$lib/assets/uber.png';
    import faqIcon from '$lib/assets/faqIcon.png';
    import {db} from '$lib/firebase';
    import { addDoc, collection, serverTimestamp } from 'firebase/firestore';
    import { track } from '@vercel/analytics';
    import { onMount } from 'svelte';

    let modalOpen = false;

    let openModal = () =>{
        modalOpen = true;
    }

    let closeModal = () =>{
        modalOpen = false;
    }

    let toggleFaq = (nth) =>{
        let allDivs = document.querySelectorAll('.faq-container');
        var children = allDivs[nth-1].children;

        children[1].children[1].style.transform = children[1].children[1].style.transform == 'rotate(180deg)' ? 'rotate(0deg)' : 'rotate(180deg)';

        children[1].classList.toggle('closed');
    }

    let joinWaitlist = async (e) =>{
        let emailInput = e.target.parentElement.children[0];
        let email = emailInput.value;

        if (!email){
            alert('Please enter an email') 
            return;
        }


        await addDoc(collection(db, "signUp"), {
            email: email, 
            timestamp: serverTimestamp()
        });
        emailInput.value = '';
        track('Join_Waitlist');
        openModal();
    }  

    onMount(()=>{
        navigator.geolocation.getCurrentPosition(async (position) => {
            await addDoc(collection(db, "locations"), {
            lon: position.coords.longitude,
            lat: position.coords.latitude,
            timestamp: serverTimestamp(),
        });
        });
    })
</script>

<div class="w-screen h-screen darkGreen flex flex-col items-center overflow-auto">
    <img class="w-full h-full mix-blend-overlay absolute z-10" src={bgOverlay} />

    {#if modalOpen}
        <div class="modal-container">
            <div class="modal rounded-3xl z-50 darkest fullOpacity absolute w-5/6 md:w-4/6 lg:w-1/2 h-1/2 top-1/2 -translate-y-1/2 px-5 py-6 flex flex-col gap-1 items-center">
                <p class="text-white text-5xl md:text-6xl medium">Thank You for Joining the Waitlist!</p>
                <p class="text-gray-400 text-xl md:text-2xl medium w-full">Exciting news is on the horizon - we're gearing up to release the full website very soon!</p>
                <button class="shadow-2xl absolute bottom-6 right-8 light-green-bg text-black transform p-4 rounded-xl self-end font-semibold" on:click={closeModal}>Close</button>
            </div>
        </div>
    {/if}

    <div class="darkest py-3 nav mt-6 rounded-full border border-gray-600 flex items-center px-7 justify-between">
        <p class="light-green-text header text-2xl">court<span class="normal-text">Booker</span></p>
        <a class="light-green-text join-link" href="">Join</a>
    </div>

    <div class="w-full px-9 md:px-20">
        <h1 class="light-green-text mt-8 md:mt-16">Say goodbye to the hassle of booking tennis courts. No planning needed.<!--<span class="flex flex-col justify-center"><div class="cool-border"><div class="inner-cool-border"></div></div> hard as rock.</span>--></h1>
        <h2 class="text-gray-400 w-full mb-8 mt-3 ">Our software automatically books courts, so you never have to to worry about securing a court ahead of others. Get early access below.</h2>
    

        <div class="relative z-20 w-full md:w-1/2 foldInput bg-transparent border border-gray-500 rounded-full flex">
            <input class="pr-2 flex-1 h-full bg-transparent z-20 md:placeholder:text-xl pl-4" type="text" placeholder="Email address">
            <button on:click={joinWaitlist} class="whitespace-nowrap z-50 h-full px-4 md:px-8 rounded-full light-green-border light-green-text darkest underline join-btn md:text-xl">
                Get Early Access
              </button>
        </div>
    </div>

    <div class="hidden md:flex flex-col w-full companiesHeight gap-16 absolute bottom-0">
        <p class="text-center text-2xl md:text-3xl semibold text-gray-400 md:-mb-8">Courts We Support</p>
        <div class="flex w-full px-10 md:px-20 flex-wrap">
            <div class="px-2 w-1/3 md:flex-1 flex items-center justify-center">
                <p class="text-2xl md:text-3xl light-green-text semibold">Latta Park</p>
            </div>
            <div class="px-2 w-1/3 md:flex-1 flex items-center justify-center">
                <p class="text-2xl md:text-3xl light-green-text semibold">Freedom Park</p>
            </div>
            <div class="px-2 w-1/3 md:flex-1 flex items-center justify-center">
                <p class="text-2xl md:text-3xl light-green-text semibold">Veterans Park</p>
            </div>
            <div class="px-2 w-1/3 md:flex-1 flex items-center justify-center">
                <p class="text-2xl md:text-3xl light-green-text semibold">Jeff Adams</p>
            </div>
            <div class="px-2 w-1/3 md:flex-1 flex items-center justify-center">
                <p class="text-2xl md:text-3xl light-green-text semibold">Many More</p>
            </div>
        </div>
    </div>
</div>

<div class="w-screen py-20 gap-24 light-green-bg flex flex-col items-center justify-center h-fit">
    <img class="w-full h-full mix-blend-overlay absolute z-10" src={bgOverlay} />
    <div class="flex md:w-5/6 second-top-height flex-col md:flex-row px-4 items-center">
        <div id="detailed-pricing" class="w-full overflow-x-auto">
            <div class="overflow-hidden">
                <div class="grid grid-cols-4 p-4 text-lg font-semibold text-gray-900 normal-bg gap-x-16">
                    <div class="flex items-center">Details</div>
                    <div>Parks and Rec Rules</div>
                    <div>Court Booker</div>
                </div>
                <div class="grid grid-cols-4 px-6 py-6 text-lg text-gray-700 border-b border-gray-200 gap-x-16">
                    <div class="text-gray-500">Booking Window</div>
                    <div class="normal-text medium text-xl">Must Reserve <span class="text-red-500">6 Days Ahead</span></div>
                    <div class="normal-text medium text-xl"><span class="text-green-800">5 Minutes</span> Ahead</div>
                </div>
                <div class="grid grid-cols-4 px-6 py-6 text-lg text-gray-700 border-b border-gray-200 gap-x-16">
                    <div class="text-gray-500">Cancellation Policy</div>
                    <div class="normal-text medium text-xl"><span class="text-red-500">10 Days</span> in Advance</div>
                    <div class="normal-text medium text-xl"><span class="text-green-800">10 Minutes</span> in Advance</div>
                </div>
                <div class="grid grid-cols-4 px-6 py-6 text-lg text-gray-700 border-b border-gray-200 gap-x-16">
                    <div class="text-gray-500">Maximum Session Length</div>
                    <div class="normal-text medium text-xl"><span class="text-red-500">1 Hour</span></div>
                    <div class="normal-text medium text-xl">No Time Limit</div>
                </div>
                <div class="grid grid-cols-4 px-6 py-6 text-lg text-gray-700 border-b border-gray-200 gap-x-16">
                    <div class="text-gray-500">Sessions Per Day</div>
                    <div class="normal-text medium text-xl">Up to <span class="text-red-500">2</span></div>
                    <div class="normal-text medium text-xl">No Limit</div>
                </div>
            </div>
        </div>
    </div>
    


    <div class="relative z-20 w-full md:w-1/2 foldInput bg-transparent border border-gray-500 rounded-full flex">
        <input class="pr-2 flex-1 h-full bg-transparent z-20 md:placeholder:text-xl pl-4" type="text" placeholder="Email address">
        <button on:click={joinWaitlist} class="whitespace-nowrap z-50 h-full px-4 md:px-8 rounded-full light-green-border light-green-text darkest underline join-btn md:text-xl">
            Get Early Access
          </button>
    </div>
</div>

<div class="w-screen h-5/6 darkGreen md:px-32 py-24 flex flex-col items-center gap-8 overflow-hidden">
    <h5 class="light-green-text faq text-4xl w-5/6 md:w-4/6">Frequently Asked Questions</h5>
    <div class="z-30 faq-container light-green-text w-5/6 md:w-4/6" on:click={()=>{toggleFaq(1)}}>
        <h6 class="faq-title text-2xl">How does CourtBooker work?</h6>
        <div class="w-full flex items-center justify-between mb-4">
            <p class="faq-answer text-md mt-3 w-5/6">CourtBooker leverages cutting-edge software to reserve tennis courts the moment they become available. We adapt to the demand, ensuring there's always a court waiting for you. Say goodbye to the frustration of needing to book courts a week ahead. CourtBooker is here to make playing tennis more convenient and affordable for busy tennis enthusiasts.</p>
            <img class="h-14 faq-icon" src={faqIcon} alt="FAQ Icon">
        </div>
        <hr>
    </div>

    <div class="z-30 faq-container light-green-text w-5/6 md:w-4/6" on:click={()=>{toggleFaq(2)}}>
        <h6 class="faq-title text-2xl">What is CourtBooker?</h6>
        <div class="w-full flex items-center justify-between mb-4 closed">
            <p class="faq-answer text-md mt-3 w-5/6">CourtBooker is your personal tennis court booking assistant in Charlotte. Forget about the hassle of planning your tennis games a week in advance. With CourtBooker, we do the hard work for you. Whether you're lacing up your shoes, making last-minute plans, or heading to the courts, CourtBooker ensures you get the court you want in minutes.</p>
            <img class="h-14 faq-icon" style="transform: rotate(180deg);" src={faqIcon} alt="FAQ Icon">
        </div>
        <hr>
    </div>

    <div class="z-30 faq-container light-green-text w-5/6 md:w-4/6" on:click={()=>{toggleFaq(3)}}>
        <h6 class="faq-title text-2xl">Why did we create CourtBooker?</h6>
        <div class="w-full flex items-center justify-between mb-4 closed">
            <p class="faq-answer text-md mt-3 w-5/6">We founded CourtBooker because we understand the challenges of fitting tennis into our busy lives. Traditional reservation systems can be a nightmare to navigate. CourtBooker empowers everyone to schedule games on their terms, making tennis accessible and enjoyable for all.</p>
            <img class="h-14 faq-icon" style="transform: rotate(180deg);" src={faqIcon} alt="FAQ Icon">
        </div>
        <hr>
    </div>

    <div class="z-30 faq-container light-green-text w-5/6 md:w-4/6" on:click={()=>{toggleFaq(4)}}>
        <h6 class="faq-title text-2xl">Where does your money go?</h6>
        <div class="w-full flex items-center justify-between mb-4 closed" >
            <p class="faq-answer text-md mt-3 w-5/6">CourtBooker charges a modest subscription fee to provide you with the convenience of booking courts just minutes before you play. The revenue generated is reinvested to reserve more courts at various parks, ensuring reliable access to tennis for everyone in our community. Your support helps us expand and improve the tennis experience for all players.</p>
            <img class="h-14 faq-icon" style="transform: rotate(180deg);" src={faqIcon} alt="FAQ Icon">
        </div>
        <hr>
    </div>

    <div class="relative z-20 w-full md:w-1/2 foldInput bg-transparent border border-gray-500 rounded-full flex">
        <input class="pr-2 flex-1 h-full bg-transparent z-20 md:placeholder:text-xl pl-4" type="text" placeholder="Email address">
        <button on:click={joinWaitlist} class="whitespace-nowrap z-50 h-full px-4 md:px-8 rounded-full light-green-border light-green-text darkest underline join-btn md:text-xl">
            Get Early Access
          </button>
    </div>
</div>




<style>
    .modal-container{
        position: fixed;
        display: flex;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.4); /* Semi-transparent black background for the blur */
        backdrop-filter: blur(5px); /* Add a blur effect to the background */
        justify-content: center;
        align-items: center;
        z-index: 50;
    }

    .modal-container div{
        z-index: 51;
    }

    footer{
        width: 100%;
    }

    .closed{
        justify-content: flex-end;
        margin-bottom: 0.5%;
        margin-top: -4%;
    }

    .closed p{
        display: none;
    }

    .faq-icon, div.w-full.flex.items-center.justify-between.mb-4{
        transition: transform 0.3s ease;
    }

    .faq-icon{
        cursor: pointer;
    }

    .activeIcon{
        transform: rotate(180deg);
    }

    hr{
        height: 1px;
        background-color: #95D5B2;
        border: none;
    }

    .faq-title, .regular{
        font-family: 'ClashGrotesk-Regular';
    }

    .faq-answer{
        font-family: 'ClashGrotesk-Extralight';
    }

    .faq, .semibold{
        font-family: 'ClashGrotesk-SemiBold';
    }

    h4{
        font-family: 'ClashGrotesk-SemiBold';
        line-height: 0.6;
    }

    .second-top-height{
        height: 85%;
    }

    .second-bottom-height{
        height: 15%;
    }

    .zero{
        -webkit-text-stroke: 1px #40916C;
        font-size: 24rem;
        line-height: 0.75;
    }

    .inputBelowZero{
        width: 88%;
    }

    .light-green-bg{
        background: #95D5B2;
    }

    .companiesHeight{
        height: 21%;
    }

    .foldInput{
        height: 3.7rem;
    }

    .medium{
        font-family: 'ClashGrotesk-Medium';
    }

    .foldInput input{
        font-family: 'ClashGrotesk-Medium';
        color: rgb(157, 170, 188);
        font-size: 1rem;
    }

    .join-btn{
        font-family: 'ClashGrotesk-SemiBold';
    }

    .light-green-border{
        border: 1px #95D5B2 solid;
    }

    .inner-cool-border{
        box-shadow: 31px 31px 31px; 
        border: 3px #95D5B2 solid; 
        filter: blur(31px);
    }

    .cool-border{
        height: 1px;
        width: 200px;
        border: 1px #95D5B2 solid;
    }

    h1{
        font-family: 'ClashGrotesk-Medium';
        font-size: 3.5rem;
        line-height: 1;
    }

    h2{
        font-size: 1.2rem;
    }

    

    @media (min-width: 768px) { 
    h1 {
        font-size: 5rem;
    }

    h2 {
        font-size: 1.5rem;
    }

    .foldInput {
        height: 3.9rem;
    }

    .foldInput input {
        font-size: 1.2rem;
    }

    .zero {
        -webkit-text-stroke: 1px #40916C;
        font-size: 5rem; /* Updated font size to be consistent */
        line-height: 0.75;
    }

    .inputBelowZero {
        width: 70%;
    }
}

@media (max-width: 900px) { 
    h1 {
        font-size: 3.3rem;
    }

    .foldInput{
        width: 90%;
    }
}


    .light-green-text{
        color: #95D5B2;
    }

    .join-link{
        text-decoration: underline;
        font-family: ClashGrotesk-Bold;
        font-size: 20px;
    }

    .header{
        font-family: ClashGrotesk-Bold;
    }

    .darkGreen{
        background: #1B4332;
    }

    .darkest{
        background: #1B152D;
        opacity: 0.8;
    }

    .fullOpacity{
        opacity: 1;
    }

    .nav{
        width: 90%;
    }

    .normal-bg{
        background: #40916C;
    }

    .normal-text{
        color: #40916C;
    }
</style>