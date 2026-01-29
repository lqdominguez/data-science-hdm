This is the most exciting part of the project! You have built a solid foundation with your preprocessing pipeline, so now you can focus on the "Model Zoo" and finding the best performer.

Here is a structured plan to tackle these requirements systematically.

---

### Phase 1: The "Baseline" Sprint

Before you optimize, you need to know the "out-of-the-box" performance of different algorithms.

1. **Prepare X and y:** Split your cleaned datasets into features () and the target ( = `cnt`).
2. **Algorithm Selection:** Initialize the models with default parameters:
* **Decision Tree:** `DecisionTreeRegressor()`
* **SVR:** `SVR()` (Note: SVR is very sensitive to scaling!)
* **k-NN:** `KNeighborsRegressor()`
* **Random Forest / XGBoost:** (Optional, but highly recommended for this dataset).

3. **The "Evaluation Table":** Create an empty DataFrame to store results (Algorithm Name, Train RMSE, Val RMSE).


#### Phase A: Stabilization (The "Fair Baseline")
Instead of jumping to optimization, simply fix the "broken" parts of your current results.
- Standardize: Create a StandardScaler pipeline for SVR, k-NN, and Linear Regression.
- Prune: Set a reasonable max_depth (e.g., 5 or 7) for all Tree models to stop the negative $R^2$ scores.
- Record: Use your record_performance function to update your "Baseline" table. You'll likely see the negative $R^2$ values disappear.
---

### Phase 2: Feature Selection (Ablation)

Even "good" features can sometimes confuse a model.

1. **Correlation Check:** Remove one of two highly correlated features (like `temp` vs `atemp`).
2. **Iterative Removal:** Try training your best model without `day_of_month` or `workingday` to see if the validation error drops.
3. **Coefficient Analysis:** For Linear models, look at the weights. For Trees, look at `feature_importances_`.

---

### Phase 3: Hyperparameter Tuning

Once you see which models are promising, try to make them better.

1. **SVR:** Experiment with `kernel='rbf'` vs `'poly'` and the `C` parameter.
2. **k-NN:** Try different values for `n_neighbors` (e.g., 3, 5, 11, 21).
3. **Trees:** Adjust `max_depth` or `min_samples_split` to prevent overfitting.
4. **Tooling:** Use `GridSearchCV` or `RandomizedSearchCV` from `sklearn` to automate this. It tests multiple combinations and finds the best one for you.

---

### Phase 4: Final Evaluation & Visualization

This is where you "prove" your work.

1. **The "Winner":** Pick the model version with the lowest **Validation RMSE**.
2. **Test Set:** Run that specific model on your `test_cleaned` data **exactly once**.
3. **Visuals:**
* **Bar Chart:** Comparison of RMSE across all models.
* **Predicted vs. Actual Plot:** A scatter plot showing how close your predictions were to the real `cnt`.
* **Feature Importance Chart:** A horizontal bar chart showing which columns the model relied on most.



---

### Phase 5: Documentation of Learnings

Keep a "lab notebook" as you go. Your grade will likely depend on explaining *why* things happened:

* *"SVR performed poorly until I applied StandardScaler."*
* *"k-NN was slow but accurate for low-demand days."*
* *"XGBoost's most important feature was temperature, which makes sense for bike rentals."*

---
