#' Get Sensitivity
#'
#' @export
#'
get_sensitivity <- function(x=x, ehr = ehr,cutoff_tops = c(10, 100, 1000)) {

    trigger_dts <- unique(x) %>%
        sort()

    outs <- dplyr::group_split(x,  trigger_dt, code_day) %>%
        lapply(as.data.frame) %>%
        lapply(function(dat) {
            #
            out <- merge()

            o = (!is.na(out$admit_dt))
            e = (as.numeric(out$prob))
            roc <- pROC::roc(response = o,  predictor = e, ci = T, plot= F)
            roc$code_day = unique(dat$code_day)

            #
            auc = pROC::auc(response = o,  predictor = e)  %>%
                pROC::ci() %>% as.numeric()

            #
            truth_tbl = data.frame(
                truth = ifelse(o, "admit", "not_admit") %>% factor(),
                admit = e
            )
            
            pr_auc  = yardstick::pr_auc(truth_tbl,truth, admit);  pr_auc

            roc_auc = yardstick::roc_auc( truth_tbl, truth, admit); roc_auc
   
            metrics <- lapply(cutoff_tops, function(cutoff_top){
                cutoff = sort(e, decreasing = T)[cutoff_top]
                table(e>cutoff,o)
                p = e>=cutoff
                tp = sum(o & p)
                fp = sum(!o & p)
                fn = sum(o & !p); fn
                tn = sum(!o & !p); tn
                total = tp + fp + fn + tn; total
                prevalence = sum(o)/total

                sensitivity <- recall <- tp/(tp+fn)
                specificity <- tn/(fp + tn)
                accuracy = (tp + tn) / (tp + fp + fn + tn)
                precision <- positive_predictive_value <- tp / (tp + fp)
                # tp / (tp + 0.5 * (fp + fn))
                # The F1 score is the harmonic mean of the precision and recall
                f1_score <- 2/(1/(precision) + 1/(sensitivity))

                metric =  data.frame(
                    task = prediction_task, trigger_dt = trigger_dt, code = code_second/3600/24,
                    top = cutoff_top,
                    observed = sum(o),     
                    prevalence=prevalence,      
                    expected = sum(e),   
                    o_over_e = sum(o) / sum(e),
                    
                    pr_auc = pr_auc$.estimate, roc_auc = roc_auc$.estimate,
                    auc = auc[2],           auc_ci_05 = auc[1], auc_ci_95 = auc[3],
                    tp =tp, fp = fp,         fn = fn, tn = tn,  total = tp + fp + fn + tn,
                    sensitivity = sensitivity,   specificity = specificity,
                    accuracy =  accuracy,
                    precision = precision,
                    f1_score  = f1_score
                )
            }) %>%
                do.call(rbind, .); metrics

            structure(list(
                roc = roc,
                patients = out %>%
                    dplyr::mutate(
                        prob = as.numeric(prob)
                    ) %>%
                    dplyr::arrange(dplyr::desc(prob)),

                metric =  metrics))

        })
    # oe
}
