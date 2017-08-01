# 2017_Postgrsql_tutorial

-- 1. HNU 엔터테인먼트의 부서의 코드, 이름 위치를 검색하시오
SELECT dept_code, dept_name, dept_loc FROM department;

-- 2. HNU 엔터테인먼트의 연예관계자인 코드, 이름, 관리자, 급여를 검색하시오
SELECT emp_code, emp_name, emp_mgt, emp_sal FROM employee;

-- 3. 2013년에 제작된 영화의 코드, 이름, 관람등급, 출시년월, 개봉년월을 검색하시오
SELECT mov_code, mov_name, mov_mpaa, mov_pddate FROM movie;

-- 4. 2013년에 제작된 드라마의 코드, 이름, 제작사, 방영사, 방영년월을 검색하시오
SELECT drm_code, drm_name, drm_prd, drm_brd, drm_opdate FROM drama;

-- 5. 2013년에 제작된 음반의 코드, 이름, 출시일자, 가격을 검색하시오
SELECT msc_code, msc_name, msc_date, msc_price FROM music;

-- 6. HNU-E 회사에서 제작한 드라마의 코드와 이름을 검색하시오
SELECT drm_code, drm_name FROM drama WHERE drm_prd='HNU-E';

-- 7. 음반 구분이 싱글인 음반의 코드, 이름, 출시일자를 검색하시오
SELECT msc_code, msc_name, msc_date FROM music WHERE msc_csf='싱글';

-- 8. 음반 구분이 정규의 코드, 이름, 출시일자, 가격을 검색하시오
SELECT msc_code, msc_name, msc_date, msc_date FROM music WHERE msc_csf='정규';

-- 9. 2013년 3월 이후에 출시된 영화의 이름, 관람등급을 검색하시오
SELECT mov_name, mov_mpaa FROM movie WHERE mov_pddate>='2013-03-01';

-- 10. 음반 가격이 만원 이상인 음반의 이름과 가격을 검색하시오
SELECT msc_name, msc_price FROM music WHERE msc_price >= 10000;

-- 11. 영화 중 전체 관람 등급이나 16세 미만이 볼 수 있는 영화 코드와 이름을 검색하시오
SELECT mov_code, mov_name FROM movie WHERE mov_mpaa IN ('A', '12', '15');

-- 12. 드라마 방영사가 KBC이거나 SBC인 드라마를 검색하시오
SELECT * FROM drama WHERE drm_brd IN ('KBC', 'SBC');

-- 13. 2013년도에 드라마를 제작한 드라마제작사를 검색하시오. 단, 중복된 값이 있으면 제거하시오
SELECT drm_prd FROM drama WHERE drm_opdate BETWEEN '2013-01-01' AND '2013-12-31';

-- 14. 연예관계자들의 급여의 총합과 평균 급여액을 계산하시오
SELECT SUM(emp_sal) AS SUMSAL, AVG(emp_sal) AS AVGSAL FROM employee;

-- 15. 아직 방영일자가 확정되지 않은 드라마의 이름을 검색하시오
SELECT drm_name FROM drama WHERE drm_opdate IS NULL;

-- 16. 방영일자가 확정되지 않은 드라마의 방영일자가 '2013-05-01'로 편성되었습니다 방영일자를 편성일에 맞게 변경하시오
UPDATE drama SET drm_opdate = '2013-05-01' WHERE drm_opdate IS NULL;

-- 17. 연예관계자의 이름과 현재 직급을 검색하시오
SELECT E.emp_name, ER.emp_rname FROM employee E, emp_role ER WHERE E.emp_rcode = ER.emp_rcode;

-- 18. 연예관계자가 소속된 부서의 이름과 연예 관계자의 이름을 검색하시오
SELECT D.dept_name, E.emp_name FROM department D, employee E, rel_department RD WHERE D.dept_code = RD.rd_dept_code AND RD.rd_emp_code = E.emp_code;

-- 19. 연예관계자가 소속된 부서의 이름과 연예 관계자의 이름을 검색하시오. 단, 부서를 기준으로 출력해야 함
SELECT D.dept_name, E.emp_name FROM department D, employee E, rel_department RD WHERE D.dept_code = RD.rd_dept_code AND RD.rd_emp_code = E.emp_code ORDER BY D.dept_name;

-- 20. 연예 관계자에 대해 연예관계자의 이름과 직속 상사의 이름을 검색하시오
SELECT E.emp_name, E2.emp_name FROM employee E, employee E2 WHERE E.emp_mgt = E2.emp_code;

-- 21. 모든 연예관계자에 대해 이름과 급여를 출력하고 급여의 내림차순으로 정렬하시오. 단, 동일한 급여액일 때 이름의 오름차순으로 정렬해야 함
SELECT emp_name, emp_sal FROM employee ORDER BY emp_sal DESC, emp_name;

-- 22. 부서별로 모든 연예관계자에 대해 이름과 급여를 출력하고, 급여의 내림차순으로 정렬하시오. 단, 동일한 급여액일 때 이름의 오름차순으로 정렬해야 함
SELECT emp_name, emp_sal FROM employee ORDER BY emp_rcode, emp_sal DESC, emp_name;

-- 23. 배우와 가수를 겸업하는 연예관계자의 이름을 검색하시오
SELECT E.emp_name FROM employee E, department D, rel_department RD WHERE E.emp_code = RD.rd_emp_code AND D.dept_code = RD.rd_dept_code AND D.dept_name='배우'
INTERSECT
SELECT E.emp_name FROM employee E, department D, rel_department RD WHERE E.emp_code = RD.rd_emp_code AND D.dept_code = RD.rd_dept_code AND D.dept_name LIKE '가수%';

SELECT A.emp_name
FROM ( SELECT E.emp_name FROM employee E, department D, rel_department RD WHERE E.emp_code = RD.rd_emp_code AND D.dept_code = RD.rd_dept_code AND D.dept_name='배우') A,
     ( SELECT E.emp_name FROM employee E, department D, rel_department RD WHERE E.emp_code = RD.rd_emp_code AND D.dept_code = RD.rd_dept_code AND D.dept_name LIKE '가수%') S
WHERE A.emp_name = S.emp_name;

-- 24. 영화에서 주연을 맡은 연예관계자의 이름과 영화 이름을 검색하시오
SELECT E.emp_name, M.mov_name FROM employee E, movie M, part_movie PM WHERE E.emp_code = PM.pm_emp_code AND M.mov_code = PM.pm_mov_code AND PM.pm_emp_role='주연';

-- 25. 모든 연예관계자에 대해 직급별로 그룹화하고, 평균 급여액이 5000 이상인 직급에 대해 연예관계자의 직급과 평균 급여액, 최소 급여액, 최대 급여액을 검색하라
SELECT ER.emp_rname, AVG(E.emp_sal), MIN(E.emp_sal), MAX(E.emp_sal) FROM employee E, emp_role ER WHERE E.emp_rcode = ER.emp_rcode GROUP BY ER.emp_rname HAVING AVG(E.emp_sal) >= 5000;

-- 26. 모든 연예관계자의 평균 급여액보다 많은 급여를 받는 연예관계자의 이름과 급여액을 검색하라
SELECT emp_name, emp_sal FROM employee WHERE emp_sal > (SELECT AVG(emp_sal) FROM employee);

-- 27. 모든 연예관계자 중 직급이 엔터테이너가 아닌 연예관계자의 이름을 검색하라
SELECT E.emp_name FROM employee E, emp_role ER WHERE E.emp_rcode = ER.emp_rcode AND ER.emp_rname NOT IN ('엔터테이너');

SELECT emp_name FROM employee E WHERE NOT EXISTS (SELECT * FROM emp_role ER WHERE E.emp_rcode = ER.emp_rcode AND emp_rname='엔터테이너');

-- 28. 연예관계자의 김수현 씨가 대리에서 실장으로 승진하고 급여가 20% 증가했습니다. 이에 알맞게 연예관계자 테이블을 수정하시오
UPDATE employee SET emp_rcode='R003', emp_sal = emp_sal*1.2 WHERE emp_name='김수현';
SELECT * FROM employee;

-- 29. 우리 회사의 엔터테이너들이 2013년에 출연하거나 출연 예정인 모든 작품의 이름을 검색하시오 (UNION)
SELECT DISTINCT M.mov_name FROM movie M, part_movie PM WHERE M.mov_code = PM.pm_mov_code
UNION
SELECT DISTINCT M.msc_name FROM music M, part_music PM WHERE M.msc_code = PM.pm_msc_code
UNION
SELECT DISTINCT D.drm_name FROM drama D, part_drama PD WHERE D.drm_code = PD.pd_drm_code;
