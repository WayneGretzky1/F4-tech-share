* vars you will need:

    const question = ref('Should the Minnesota Twins keep Pablo Lopez?')
    const typed = ref('')
    let controls


- question stores the full text.

- typed holds the portion currently displayed as the animation progresses.


-------------------------------------------------


* how to run the animation:


    onMounted(() => {
        const text = question.value
        controls = animate(0, text.length, {
            duration: text.length * 0.12, 
            ease: 'linear',
            onUpdate: (v) => {
            typed.value = text.slice(0, Math.floor(v))
            }
        })
        })



* in the template it will look like this

    <h2 class="content-head is-center">
        <span>{{ typed }}</span><span class="cursor">|</span>
    </h2>



* to make the cursor blink

    .cursor {
        display: inline-block;
        animation: blink 1s steps(1) infinite;
    }
    @keyframes blink {
      50% { opacity: 0; }
    }
