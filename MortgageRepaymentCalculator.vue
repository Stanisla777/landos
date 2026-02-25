this.$nextTick(()=>{
      console.log('mounted')
      setTimeout(()=>{
        contentNote();
      },500)

    })
