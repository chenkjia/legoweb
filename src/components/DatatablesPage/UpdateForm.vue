<script>
import { omit, cloneDeep } from 'lodash'

export default {
  props: {
    updateList: {
      type: Array,
      default: () => ([])
    }
  },
  data () {
    return {
      updateFormId: null,
      updateData: {},
      updateDialogVisible: false,
      updateButtonList: [{
        label: '取 消',
        func: () => {
          this.updateDialogVisible = false
        }
      }, {
        label: '确 定',
        type: 'primary',
        func: this.updateFormSubmit
      }]
    }
  },
  computed: {
    updateFields () {
      return this.updateList.map(item => {
        const field = cloneDeep(this.fields[item])
        return {
          name: item,
          ...field,
          form: {
            ...field.form,
            ...field.update
          }
        }
      })
    }
  },
  methods: {
    updateFormInit (data) {
      this.updateFormId = data._id
      this.updateData = omit(data, ['_id'])
      this.updateDialogVisible = true
    },
    updateFormSubmit (data) {
      this.$updateOne({
        url: this.resource,
        params: {
          _id: this.updateFormId
        },
        data
      }).then(res => {
        if (res.code === 0) {
          this.updateDialogVisible = false
          this.getList()
        }
      })
    }
  }
}
</script>
