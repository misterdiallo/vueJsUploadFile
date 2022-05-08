<template>
  <div style="text-align: center;">
    <el-upload
      class="upload-demo"
      action=""
      :limit="1"
      :file-list="fileList"
      :http-request="uploadfile"
    > 
      <el-button size="small" type="primary" style=" background-color: RGB(71, 63, 151); font-size: large;">上传文件
      </el-button>
    </el-upload>
  </div>
</template>

<script>

  // import utils from '@/utils'
  // const url='http://120.48.24.241/'
  const url = 'http://wisdompc.lyu.edu.cn/users-service/v0/api/file/add'
  // const url='http://www.sdhxzn.cn/'
  export default {
    data() {
      return {
        fileList:[],
        formInline: {
          userId: '',
          file: '',
        },
      }
    },

    created() {
      let userId = this.getUrlKey('userId').split('?')
      this.getParams(userId)
    },

    methods: {
      getUrlKey: function (name) {
        console.log(name)
        var str = (new RegExp('[?|&|/]' + name + '=' + '([^&;]+?)(&|#|;|$)').exec(location.href) || [""])[1]
        console.log(decodeURIComponent(str))
        return decodeURIComponent(str.replace(/\+/g, '%20')) || null;
      },
      getParams(data1) {
        console.log(data1)
        //接收函数
        this.formInline.userId = data1
      },
      uploadfile(file) {
        let fd = new FormData()
        console.log(file.file)
        console.log("file extension: " + "." + file.file.name.substr(file.file.name.lastIndexOf('.') +1))
        console.log("file name: " + file.file.name)
        console.log("file type: " + file.file.type)
        console.log("user Id: " + this.formInline.userId)
        fd.append('file', file.file)
        fd.append('fileName', file.file.name)
        fd.append('fileType', file.file.type)
        fd.append('fileExtension', "." + file.file.name.substr(file.file.name.lastIndexOf('.') +1))
        fd.append('userId', this.formInline.userId)

        this.axios({
          method: "POST",
          url: url,
          headers: {
            'Content-Type': 'multipart/form-data',
            // 'token': 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIyMDE1MDkzMTA1MjYiLCJleHAiOjE2NTEyMzE5MDQsImlhdCI6MTY1MTIyNDcwNH0.Y0VTLo2a6bL7U95AEtTn9XnJM4SL4inIWNVCI5YTT-o',
          },
          data: fd,
        })
          .then(res => {
            console.log(res)
            console.log(res.status);
            if (res.status != 200) {
              this.$message('上传失败' + res.extend.msg, 'error')
              this.fileList = []
            } else {
              this.$message('上传成功', 'success')
              // this.fetchData();
            }
          })
          .catch(err => {
            console.log(err)
            this.$message.error('上传失败，请添加文件后重试！')
          })

      },
    }

  }
</script>
<style>
  .el-pagination {
    position: absolute;
    left: 30vw;
    bottom: 0px;
  }

  .fromHeader .el-select .el-input {
    border-color: #409EFF;
    width: 200px;
  }

  .demo-table-expand {
    font-size: 0;
  }

  .user-search {
    margin-top: 20px;
  }

  .demo-table-expand label {
    width: 90px;
    color: #99a9bf;
  }

  .demo-table-expand .el-form-item {
    margin-right: 0;
    margin-bottom: 0;
    width: 100%;
  }

  .el-form-item__content {
    width: 100%;
  }

  /* 隐藏上方表格多余部分 */
  .undefined.el-table__expand-column {
    display: none;
  }

  /* 隐藏上方表格多余部分 */
  .el-table_1_column_8 .el-table--border td, .el-table--border th, .el-table__body-wrapper .el-table--border.is-scrolling-left ~ .el-table__fixed {
    border-right: 0px solid #ebeef5
  }

  .tablerowclass {
    background: #EAEAEA !important;
  }

  /* 去掉中间数据的分割线 */
  .el-table__row > td {
    border-right: none !important;

  }

  .el-table > tr {
    border-right: none !important;
    border-left: none !important;
  }

  .tableclass1 {
    border: none !important;
  }


</style>
