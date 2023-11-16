#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include "aes.h"
#define min(a,b) (((a)<(b))?(a):(b))

unsigned char *aes128_func(unsigned char *in, unsigned char *aes_key, char type, unsigned int *len)
{
    unsigned char ptext[KEY_128]={0},ctext[KEY_128]={0},*out=NULL;
    unsigned char *temp_in=NULL,*temp_out=NULL;
    aes_ctx_t *ctx = NULL;
    if(!in || !len || !(aes_key && (strlen(aes_key) < 17))) return NULL;
    init_aes();
    if(!(ctx = aes_alloc_ctx(aes_key, KEY_128))) {
            return NULL;
    }
    memset(ctx, 0, sizeof(*ctx));
    *len = *len + ( (*len % 16) ? (16 - (*len % 16)): 0);


    if(!(out =(char *)malloc(sizeof(*len+1))))
    {
            aes_free_ctx(ctx);
            return NULL;
    }

    memset(out,0,*len+1);
    temp_in=in;
    temp_out=out;
    do{
        memset(ptext, 0, sizeof(ptext));
            memset(ctext, 0, sizeof(ctext));
            if(type==0){
                    memcpy(ptext, temp_in, min(sizeof(ptext),strlen(temp_in)));
                    aes_encrypt(ctx, ptext, ctext);
            }else{
                    memcpy(ptext, temp_in, sizeof(ptext));
                    aes_decrypt(ctx, ptext, ctext);
            }
            memcpy(temp_out,ctext,KEY_128);

    }while(((temp_in += KEY_128) < in+*len) && ((temp_out += KEY_128) < out+*len));

    if(ctx) aes_free_ctx(ctx);
    if(type==1) *len=strlen(out);
    printf ("\nout aes %s\n",out);
    return out;
}


unsigned char *process_post_data(unsigned char *in, unsigned char *aes_key, char type, unsigned int *ret_len)
  {
    unsigned char *step1_buf=NULL, *step2_buf=NULL;
    unsigned int len=0;
    printf ("at process_post_data \nin=%s\t aes_key=%s\t type=%s\t ret_len=%u\n", in, aes_key, type, ret_len);
    if(!in || !aes_key) return NULL;
    len = strlen(in);

    if(type == 0)
    {
        if(!(step1_buf = aes128_func(in, aes_key, type, &len)))
            return NULL;
        printf ("\naes enc data is \"%s\" \n",step1_buf);
    }
}


int main(int argc, char *argv[])
{
      unsigned int ret_len;
      char *data=NULL, *processed_data=NULL, *post_data=NULL;
      data="{\"RequestUniqueID\":\"987654321\",\"MethodName\":\"DstGenerateSessionID\"}";
      printf ("data is \t %s\n ",data);
      if(!(processed_data = process_post_data(data,"1848519011345614",0,&ret_len)))
      {
          printf ("sorry something worng try again\n");
      }
}
