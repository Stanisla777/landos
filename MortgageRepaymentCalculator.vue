const indexArray = this.block_requared.find(obj => obj.id === array_data[1]);
      if (indexArray) {
        this.$set(indexArray,'date_end',true)
      }
