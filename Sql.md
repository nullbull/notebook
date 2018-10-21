select count(*) from employee_info_modify_record where channel = 1；

 select count(distinct user_id) as totalPersCount, count(*) as totalWorkOrderNum from employee_info_modify_record where channel = 3
 and  create_time between '2018-08-02 19:00:00' and '2018-08-02 23:00:53';

更新标签和备注

update work_order_info set tag_id =3 where work_order_id = 1745

update work_order set pending_comment ='test20180813' where id = 1;



```
UPDATE work_order set pending_comment = 'ttest' where id = 1
```

```
/**
 * 更新标签及备注
 * @param SpecialWorkOrderUpdateTagVo
 * @return
 */
@Override
public ReturnData updateTagAndComment(SpecialWorkOrderUpdateTagVo tagVo) {
    String methodDesc = "更新服务单标签及其备注";
    logger.info(LogBuild.getBuilder(methodDesc,"开始").appendPV("SpecialWorkOrderUpdateTagVo", JSON.toJSONString(tagVo)).build());
    ReturnData rd = getReturnData();

    try {
        if (tagVo == null) {
            logger.info(LogBuild.getBuilder(methodDesc, "对象为null").appendPV("SpecialWorkOrderUpdateTagVo", JSON.toJSONString(tagVo)).build());
            rd.setMsg("传入参数为空");
            return rd;
        }
        /*数据获取*/
        Long workOrderId = tagVo.getWorkOrderId();
        Long tagId = tagVo.getWorkOrderId();
        String penddingComment = tagVo.getPendingComment();

        if (workOrderId < 0 || workOrderId == null) {
            logger.info(LogBuild.getBuilder(methodDesc, "服务单号不正确").appendPV("workOrderId", workOrderId).build());
            rd.setMsg("服务单号不正确");
            return rd;
        }
        if(tagId < 0 || tagId == null) {
            logger.info(LogBuild.getBuilder(methodDesc, "标签设置不正确").appendPV("tagId", tagId).build());
            rd.setMsg("标签设置不正确");
            return rd;
        }
        /*更新TagId*/
        int mark = workOrderInfoMapper.updateTagId(workOrderId, tagId);
        /*如果penddingComment备注不为空，进行更新*/
        if (mark != 1) {
            rd.setMsg("更新失败，服务单不存在");
            logger.info(LogBuild.getBuilder(methodDesc, "服务单不存在").appendPV("workOrder", workOrderId).build());
            return rd;
        }
        if (penddingComment != null && StringUtils.isNotEmpty(penddingComment)) {
            workOrderMapper.updatePendComment(workOrderId, penddingComment);
        }
        rd.setMsg("操作成功");
        rd.setRetCode(CommonConstants.RETURN_CODE_SUCCESS);
        logger.info(LogBuild.getBuilder(methodDesc,"完成").kv("tagVo",JSON.toJSONString(tagVo)).build());
        return rd;

    } catch (Exception e) {
        rd.setMsg("更新失败，出现异常");
        logger.info(LogBuild.getBuilder(methodDesc,"失败").appendPV("exception",e.toString()).build(),e);
        return rd;

    }

}
```
```
@RequestMapping(value = "/update", method = RequestMethod.POST)
public ReturnData ttes(String params) {
    String methodDesc = "更新服务单标签及其备注接口";
    ReturnData rd = new ReturnData();
    logger.info(LogBuild.getBuilder(methodDesc, "开始").kv("params", params).build());
    rd.setRetCode(CommonConstants.RETURN_CODE_FAIL);
    try {
        if (params == null || StringUtils.isEmpty(params)) {
            logger.info(LogBuild.getBuilder(methodDesc, "参数为空").kv("params", params).build());
        }
        SpecialWorkOrderUpdateTagVo tagVo = JSON.parseObject(params, SpecialWorkOrderUpdateTagVo.class);
        if (tagVo.getWorkOrderId() == null) tagVo.setWorkOrderId((long) -1);
        if (tagVo.getTagId() == null) tagVo.setTagId((long) -1);
        if (tagVo.getPendingComment() == null) tagVo.setPendingComment("");
        rd = workOrderSpecialService.updateTagAndComment(tagVo);
        logger.info(LogBuild.getBuilder(methodDesc, "完成").kv("params", params).build());
        return rd;

    } catch (Exception e) {
        logger.info(LogBuild.getBuilder(methodDesc,"失败").kv("exception",e.toString()).build(),e);
        rd.setMsg("更新失败，出现异常");
        return rd;
    }

}
```



一个子女险的保险列表中可能包含多个子女，需要核对一下多个子女生成的服务列表的状态是否正确，因此需要将符合以下条件的保险列表导出来 a 将保险列表中，保险是医疗3子女（0/50%）、医疗4-5子女（0/50%）的、子女数＞1的保险列表导出一个excel b excel字段包括：保险列表ID、主状态、子状态、开始日期、结束日期；

SELECT id, STATUS, sub_status, create_time, end_time FROM employee_insurance WHERE id IN( SELECT employee_insurance_id FROM employee_insurance_children WHERE employee_insurance_id IN( SELECT id FROM employee_insurance WHERE insurance_id = 24 OR insurance_id = 8) GROUP BY employee_insurance_id HAVING COUNT(*) > 1)



SELECT id, STATUS, sub_status, create_time, end_time FROM employee_insurance WHERE insurance_id IN(24, 8) AND id IN (SELECT employee_insurance_id FROM employee_insurance_children GROUP BY employee_insurance_id HAVING COUNT(*) > 1) ORDER BY id;

排序

select commodity_id, commodity_price, case when @prevRank = commodity_price then @rownum when @preRank := commodity_price
 then @rownum :=@rownum +1 end AS rownum from sm_commodity a, (select @rownum := 0, @preRank := NULL) q order by commodity_price desc;



select commodity_id, commodity_price, case when @prevRank = commodity_price then @rownum when @preRank := commodity_price then @rownum :=@rownum +1 end AS rownum from sm_commodity a, (select @rownum := 0, @preRank := NULL) q order by commodity_price desc;